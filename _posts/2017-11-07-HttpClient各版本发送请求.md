---
layout: post
title: "HttpClient各版本发送请求"
description: ""
category: 
tags: [Java]
---


## HttpClient大致分为三个版本：
- HttpClient 3.x
- HttpClient 4.x -- HttpClient 4.3
- HttpClient 4.3 及以上

### HttpClient 3.x

```java
HttpClient client = new HttpClient();  
HttpMethodmethod = new GetMethod("http://java.sun.com");  
// 使用POST方法  
// HttpMethod method = new PostMethod("http://java.sun.com");  
client.executeMethod(method);  
// 打印服务器返回的状态  
System.out.println(method.getStatusLine());  
// 打印返回的信息  
System.out.println(method.getResponseBodyAsString());  
// 释放连接  
method.releaseConnection();  
```

### HttpClient 4.x -- HttpClient 4.3

```java
public void getUrl(String url, String encoding) throws ClientProtocolException, IOException {  
    HttpClient client = new DefaultHttpClient();  
    HttpGet get = new HttpGet(url);  
    //设置超时时间  
    HttpParams params = client.getParams();  
    HttpConnectionParams.setConnectionTimeout(params, (int) 10 * 1000);  
    HttpConnectionParams.setSoTimeout(params, 10 * 1000);  
    HttpResponse response = client.execute(get);  
    HttpEntity entity = response.getEntity();  
    if (entity != null) {  
        InputStream instream = entity.getContent();  
        try {  
            BufferedReader reader = new BufferedReader(new InputStreamReader(instream, encoding));  
                System.out.println(reader.readLine());  
            } catch (Exception e) {  
                e.printStackTrace();  
            } finally {  
                instream.close();  
            }  
        }  
        // 关闭连接.  
        client.getConnectionManager().shutdown();  
    }  
```


### HttpClient 4.3 及以上

```java
import org.apache.http.HttpEntity;  
import org.apache.http.HttpResponse;  
import org.apache.http.client.methods.HttpGet;  
import org.apache.http.impl.client.CloseableHttpClient;  
import org.apache.http.impl.client.HttpClients;  
import org.apache.http.util.EntityUtils;  
  
  
public static String getResult(String urlStr) {  
    CloseableHttpClient httpClient = HttpClients.createDefault();  
    // HTTP Get请求  
    HttpGet httpGet = new HttpGet(urlStr);  
    // 设置请求和传输超时时间  
    // RequestConfig requestConfig =  
    // RequestConfig.custom().setSocketTimeout(TIME_OUT).setConnectTimeout(TIME_OUT).build();  
    // httpGet.setConfig(requestConfig);  
    String res = "";  
    try {  
        // 执行请求  
        HttpResponse getAddrResp = httpClient.execute(httpGet);  
        HttpEntity entity = getAddrResp.getEntity();  
        if (entity != null) {  
            res = EntityUtils.toString(entity);  
        }  
            log.info("响应" + getAddrResp.getStatusLine());  
        } catch (Exception e) {  
            log.error(e.getMessage(), e);  
            return res;  
        } finally {  
            try {  
                httpClient.close();  
            } catch (IOException e) {  
                log.error(e.getMessage(), e);  
                return res;  
            }  
        }  
        return res;  
    }  
```



{% include JB/setup %}
