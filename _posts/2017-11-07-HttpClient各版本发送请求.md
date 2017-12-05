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


## 各版本注意事项

- 无论是哪个版本，那种请求方式类（HttpPost, HttpGet, PostMethod, GetMethod）,在传递参数的时候一定要注意加上参数编码

```JAVA
PostMethod postMethod = new PostMethod();
...
List<NameValuePair> nvps = new ArrayList<NameValuePair>();
...
httpPost.setEntity(new UrlEncodedFormEntity(nvps, charset));
```


- 导包，添加maven依赖的时候要注意，其中3.X版本的是`org.apache.commons.httpclient.HttpClient`, maven仓库上最高版本`3.1`，(截止`Aug, 2007`已经不再维护)

- `org.apache.http.client.HttpClient`继续在之前的版本维护，第一个版本为`4.0xx`，4.0以后的版本支持SSL


---


## 4.0 - 4.3 示例

```java
public class HttpClientT {
	
	private static String error_result = "Request Error";

	public static String doPostStr(String url, String params, String charset) {
		String result = "";
		HttpClient httpClient = new DefaultHttpClient();
		HttpUriRequest httpPost = null;
		try {
			httpPost = new HttpPost(url);
			((HttpPost)httpPost).setEntity(new StringEntity(params, charset));
			setHeaderParams(httpPost, charset);
			HttpResponse response = httpClient.execute(httpPost);
			if (response.getStatusLine().getStatusCode() == HttpStatus.SC_OK) {
				HttpEntity resultEntity = response.getEntity();
				result = EntityUtils.toString(resultEntity);
			}
		} catch (IOException e) {
			e.printStackTrace();
			result = error_result;
		}
		return result;
	}
	
	public static String doPost(String url, Map<String, String> params, String charset) {
		HttpClient httpClient = new DefaultHttpClient();
		String result = "";
		
		HttpPost httpPost = null;
		List<NameValuePair> nvps = new ArrayList<NameValuePair>();

		if (null != params) {
			for (Map.Entry<String, String> item : params.entrySet()) {
				nvps.add(new BasicNameValuePair((String) item.getKey(), (String) item.getValue()));
			}
		}
		try {
			httpPost = new HttpPost(url);
			httpPost.setEntity(new UrlEncodedFormEntity(nvps, charset));
			setHeaderParams(httpPost, charset);
			HttpResponse response = httpClient.execute(httpPost);
			if (response.getStatusLine().getStatusCode() == HttpStatus.SC_OK) {
				HttpEntity resultEntity = response.getEntity();
				result = EntityUtils.toString(resultEntity);
			}
		} catch (IOException e) {
			e.printStackTrace();
			result = error_result;
		}
		return result;
	}
	
	public static void setHeaderParams(HttpUriRequest req, String charset) {
		req.setHeader("Content-type", "application/x-www-form-urlencoded; charset=" + charset);
		req.setHeader("Accept-Language", "zh-cn");
		req.setHeader("Cache-Control", "no-cache");
		req.setHeader("Connection", "Close");
	}
}


```

---

{% include JB/setup %}
