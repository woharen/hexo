---
title: SpringBoot上传文件显示上传进度
date: 2021年3月26日 09:50:24
tags:
  - SpringBoot
---

1. 创建maven依赖

```java
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.2.2</version>
    </dependency>
```

1. 新建实体类

```java
@Data
@EqualsAndHashCode(callSuper = false)
@Accessors(chain = true)
public class ProgressEntity {

    /**
     * 读取的文件的比特数
     */
    private long bytesRead = 0L;

    /**
     * 文件的总大小
      */
    private long contentLength = 0L;

    /**
     * 目前正在读取第几个文件
     */
    private int items;

    /**
     *  开始上传时间，用于计算上传速率
     */
    private long startTime = System.currentTimeMillis();

}
123456789101112131415161718192021222324252627
```

1. 新建监听器

```java
/**
 *
 * @author Administrator
 *
 * 要获得上传文件的实时详细信息，必须继承org.apache.commons.fileupload.ProgressListener类，
 * 获得信息的时候将进度条对象Progress放在该监听器的session对象中
 */
@Component
public class FileUploadProgressListener implements ProgressListener {

    private HttpSession session;

    public void setSession(HttpSession session){
        this.session=session;
        //保存上传状态
        ProgressEntity progressEntity = new ProgressEntity();
        session.setAttribute("progressEntity", progressEntity);
    }

    @Override
    public void update(long bytesRead, long contentLength, int items) {
        ProgressEntity progressEntity = (ProgressEntity) session.getAttribute("progressEntity");
        //已读取数据长度
        progressEntity.setBytesRead(bytesRead);
        //文件总长度
        progressEntity.setContentLength(contentLength);
        //正在保存第几个文件
        progressEntity.setItems(items);
    }
}

12345678910111213141516171819202122232425262728293031
```

1. 新建文件解析器

```java
/**
 * @author Administrator
 *
 * SpringMVC默认有一个文件解析器CommonsMultipartResolver用来解析上传的文件，我们需要重写该类，
 * 自己重写的监听器放到org.apache.commons.fileupload.FileUpload中，还需要将session放到自定义的监听器中
 */

public class CustomMultipartResolver extends CommonsMultipartResolver {

    /**
     * 注入第二步写的FileUploadProgressListener
     * 此处一定要检查是否注入成功了，不成功的话会报错
     */
    @Autowired
    private FileUploadProgressListener progressListener;

    public void setProgressListener(FileUploadProgressListener progressListener) {
        this.progressListener = progressListener;
    }

    @Override
    public MultipartParsingResult parseRequest(HttpServletRequest request) throws MultipartException {
        String encoding = determineEncoding(request);
        FileUpload fileUpload = prepareFileUpload(encoding);
        progressListener.setSession(request.getSession());
        fileUpload.setProgressListener(progressListener);
        try {
            List<FileItem> fileItems = ((ServletFileUpload) fileUpload).parseRequest(request);
            return parseFileItems(fileItems, encoding);
        } catch (FileUploadBase.SizeLimitExceededException ex) {
            throw new MaxUploadSizeExceededException(fileUpload.getSizeMax(), ex);
        } catch (FileUploadException ex) {
            throw new MultipartException("Could not parse multipart servlet request", ex);
        }
    }

}
```

1. 指定自定义解析器

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    /**
     * 指定自定义解析器
     * 将 multipartResolver 指向我们刚刚创建好的继承 CustomMultipartResolver 类的 自定义文件上传处理类
     *
     * @return
     */
    @Bean(name = "multipartResolver")
    public MultipartResolver multipartResolver() {
        return new CustomMultipartResolver();
    }
}
```

1. 控制器调用方法

```java
 /**
     * 获取上传进度
     *
     * @return
     */
@GetMapping(value = "/uploadStatus")
@ApiOperation("获取上传进度")
public Object uploadStatus() {
    HttpServletRequest request = HttpContextUtils.getHttpServletRequest();
    HttpSession session = request.getSession();
    ProgressEntity percent = (ProgressEntity) session.getAttribute("progressEntity");
    // System.out.println("^^^^"+percent.toString());
    //当前时间
    long nowTime = System.currentTimeMillis();
    //已传输的时间
    long time = (nowTime - percent.getStartTime())/1000+1;
    //传输速度 ;单位：byte/秒
    double velocity =((double) percent.getBytesRead())/(double)time;
    //估计总时间
    double totalTime = percent.getContentLength()/velocity;
    //估计剩余时间
    double timeLeft = totalTime - time;
    //已经完成的百分比
    int percent1 = (int)(100 * (double) percent.getBytesRead() / (double) percent.getContentLength());
    //已经完成数单位:m
    double length = ((double) percent.getBytesRead())/1024/1024;
    //总长度  M
    double totalLength = (double) percent.getContentLength()/1024/1024;
    StringBuffer sb = new StringBuffer();
    //拼接上传信息传递到jsp中
    sb.append(percent+":"+length+":"+totalLength+":"+velocity+":"+time+":"+totalTime+":"+timeLeft+":"+percent.getItems());
    System.out.println("百分比====>"+percent1);
    System.out.println("已传输时间====>"+time+"------剩余时间--->"+timeLeft+"======已经完成===="+length);
    return null;
}
```

原文链接：https://blog.csdn.net/FurtherSkyQ/article/details/98200965