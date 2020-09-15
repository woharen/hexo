## xml文件读取```java

package org.apache.ranger.plugin.util;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.InputStream;
import java.util.Map;

import javax.xml.XMLConstants;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import org.apache.log4j.Logger;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

public class XMLUtils {

	private static final Logger LOG = Logger.getLogger(XMLUtils.class);

	private static final String XMLCONFIG_PROPERTY_TAGNAME = "property";
	private static final String XMLCONFIG_NAME_TAGNAME = "name";
	private static final String XMLCONFIG_VALUE_TAGNAME = "value";

	/**
	 * 读取
	 * @param configFileName 配置文件路径
	 * @param properties 集合
	 */
	public static void loadConfig(String configFileName, Map<Object, Object> properties) {
		try (InputStream input = getFileInputStream(configFileName)) {
			loadConfig(input, properties);
		} catch (Exception e) {
			LOG.error("Error loading : ", e);
		}
	}

	public static void loadConfig(InputStream input, Map<Object, Object> properties) {
		try {
			DocumentBuilderFactory xmlDocumentBuilderFactory = DocumentBuilderFactory.newInstance();
			xmlDocumentBuilderFactory.setFeature(XMLConstants.FEATURE_SECURE_PROCESSING, true);
			xmlDocumentBuilderFactory.setFeature("http://xml.org/sax/features/external-general-entities", false);
                	xmlDocumentBuilderFactory.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
			xmlDocumentBuilderFactory.setIgnoringComments(true);
			xmlDocumentBuilderFactory.setNamespaceAware(true);

			DocumentBuilder xmlDocumentBuilder = xmlDocumentBuilderFactory.newDocumentBuilder();
			Document xmlDocument = xmlDocumentBuilder.parse(input);
			xmlDocument.getDocumentElement().normalize();

			NodeList nList = xmlDocument.getElementsByTagName(XMLCONFIG_PROPERTY_TAGNAME);

			for (int temp = 0; temp < nList.getLength(); temp++) {

				Node nNode = nList.item(temp);

				if (nNode.getNodeType() == Node.ELEMENT_NODE) {

					Element eElement = (Element) nNode;

					String propertyName = "";
					String propertyValue = "";
					if (eElement.getElementsByTagName(XMLCONFIG_NAME_TAGNAME).item(0) != null) {
						propertyName = eElement.getElementsByTagName(XMLCONFIG_NAME_TAGNAME)
								.item(0).getTextContent().trim();
					}
					if (eElement.getElementsByTagName(XMLCONFIG_VALUE_TAGNAME).item(0) != null) {
						propertyValue = eElement.getElementsByTagName(XMLCONFIG_VALUE_TAGNAME)
								.item(0).getTextContent().trim();
					}

					if (properties.get(propertyName) != null) {
						properties.remove(propertyName);
					}

					properties.put(propertyName, propertyValue);

				}
			}

		} catch (Exception e) {
			LOG.error("Error loading : ", e);
		}
	}

	private static InputStream getFileInputStream(String path) throws FileNotFoundException {

		InputStream ret = null;

		// Guard against path traversal attacks
		String sanitizedPath = new File(path).getName();
		if ("".equals(sanitizedPath)) {
			return null;
		}
		File f = new File(sanitizedPath);

		if (f.exists()) {
			ret = new FileInputStream(f);
		} else {
			ret = XMLUtils.class.getResourceAsStream(path);

			if (ret == null) {
				if (! path.startsWith("/")) {
					ret = XMLUtils.class.getResourceAsStream("/" + path);
				}
			}

			if (ret == null) {
				ret = ClassLoader.getSystemClassLoader().getResourceAsStream(path);
				if (ret == null) {
					if (! path.startsWith("/")) {
						ret = ClassLoader.getSystemResourceAsStream("/" + path);
					}
				}
			}
		}

		return ret;
	}

}
```

xml模版
```xml
<configuration>
	<property>
		<name>xxxxx</name>
        <value>yyyy</value>
	</property>
</configuration>
```