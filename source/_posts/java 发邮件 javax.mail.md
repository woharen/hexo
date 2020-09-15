## java 发邮件 javax.mail## 依赖
```java
<dependency>
    <groupId>com.sun.mail</groupId>
    <artifactId>javax.mail</artifactId>
    <version>1.5.5</version>
</dependency>
```

## 代码演示
```java
package com.panshi.utils;

import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Date;
import java.util.Properties;

/**
 * @description: 发送邮箱验证码类
 * @author: 蒋文豪
 * @create: 2019/06/13
 */
public class SendEmail {

    public static final String SMTPSERVER = "smtp.163.com";
    public static final String SMTPPORT = "465";
    public static final String ACCOUT = "xxx@163.com";
    public static final String PWD = "密码";

    /**
     * 发送验证码
     * @param address 地址
     * @param code 验证码
     */
    public static void sendVerification(String address, String code){
        // 创建邮件配置
        Properties props = new Properties();
        props.setProperty("mail.transport.protocol", "smtp"); // 使用的协议（JavaMail规范要求）
        props.setProperty("mail.smtp.host", SMTPSERVER); // 发件人的邮箱的 SMTP 服务器地址
        props.setProperty("mail.smtp.port", SMTPPORT); //端口
        props.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
        props.setProperty("mail.smtp.auth", "true"); // 需要请求认证
        props.setProperty("mail.smtp.ssl.enable", "true");// 开启ssl

        // 根据邮件配置创建会话，注意session别导错包
        Session session = Session.getDefaultInstance(props);
        //创建邮件
        MimeMessage message = new MimeMessage(session);;
        try {
            // 发件人邮件地址, 发件人别名（收件邮箱显示）, charset编码方式
            InternetAddress fromAddress = new InternetAddress(ACCOUT,"小尾巴96", "utf-8");
            // 设置发送邮件方
            message.setFrom(fromAddress);
            //收件地址  发件人别名（自己邮箱显示）
            InternetAddress receiveAddress = new InternetAddress("xxx@qq.com", "VinhoJiang", "utf-8");
            // 设置邮件接收方
            message.setRecipient(Message.RecipientType.TO, receiveAddress);
            // 设置邮件标题
            message.setSubject("验证码", "utf-8");
            //邮件内容
            //msg.setText("您的验证码是4528");//普通文字
            String htmlText = "您的验证码是<H2 style='color:pink'>"+code+"</H2>";
            //以网页形式发送 UTF-8
            message.setContent(htmlText, "text/html;charset=UTF-8");
            // 设置显示的发件时间
            message.setSentDate(new Date());
            // 保存设置
            message.saveChanges();
        } catch (Exception e) {
            e.printStackTrace();
        }
        //获取传输通道
        Transport transport = null;
        try {
            transport = session.getTransport();
        } catch (NoSuchProviderException e) {
            e.printStackTrace();
        }
        try {
            transport.connect(SMTPSERVER,ACCOUT, PWD);
        } catch (MessagingException e) {
            e.printStackTrace();
        }
        //连接，并发送邮件
        try {
            transport.sendMessage(message, message.getAllRecipients());
        } catch (MessagingException e) {
            e.printStackTrace();
        } finally {
            try {
                transport.close();
            } catch (MessagingException e) {
                e.printStackTrace();
            }
        }

    }

}
```