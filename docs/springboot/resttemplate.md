# RestTemplate

## 整合 Httpclient5

**maven**

```xml

<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents.client5/httpclient5 -->
<dependency>
    <groupId>org.apache.httpcomponents.client5</groupId>
    <artifactId>httpclient5</artifactId>
    <version>5.3.1</version>
</dependency>
```

**gradle**

```text
// https://mvnrepository.com/artifact/org.apache.httpcomponents.client5/httpclient5
implementation("org.apache.httpcomponents.client5:httpclient5:5.3.1")

```

**Spring配置类**

```java
import org.apache.hc.client5.http.impl.classic.CloseableHttpClient;
import org.apache.hc.client5.http.impl.classic.HttpClients;
import org.apache.hc.client5.http.impl.io.PoolingHttpClientConnectionManagerBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.client.HttpComponentsClientHttpRequestFactory;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.http.converter.StringHttpMessageConverter;
import org.springframework.web.client.RestTemplate;

import java.nio.charset.StandardCharsets;
import java.util.List;

@Configuration
public class RestTemplateConfig {

    @Bean
    public RestTemplate restTemplate(){
        RestTemplate restTemplate = new RestTemplate(getRequestFactory());

        //设置字符集为UTF-8, 解决乱码问题
        List<HttpMessageConverter<?>> messageConverters = restTemplate.getMessageConverters();
        for(HttpMessageConverter<?> messageConverter:messageConverters){
            if(messageConverter instanceof StringHttpMessageConverter){
                ((StringHttpMessageConverter) messageConverter).setDefaultCharset(StandardCharsets.UTF_8);
            }
        }
        return restTemplate;
    }

    public HttpComponentsClientHttpRequestFactory getRequestFactory(){
        CloseableHttpClient httpClient = HttpClients
                .custom()
                .setConnectionManager(
                        PoolingHttpClientConnectionManagerBuilder
                                .create()
                                .build()
                )
                .build();
        HttpComponentsClientHttpRequestFactory requestFactory = new HttpComponentsClientHttpRequestFactory();
        requestFactory.setHttpClient(httpClient);
        requestFactory.setConnectionRequestTimeout(5000);
        requestFactory.setConnectTimeout(5000);
        return requestFactory;
    }

}
```