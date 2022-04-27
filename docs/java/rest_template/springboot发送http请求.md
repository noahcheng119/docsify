# SpringBoot发送Http请求
概述：本教程适应与Java后端（Spring项目）通过restTemplate发送Http请求，多用于推送数据，本教程以推送数据为例（post请求）。

## 步骤
1、创建指定字符集的RestTemplate
```java
import org.springframework.http.client.SimpleClientHttpRequestFactory;
import org.springframework.http.converter.StringHttpMessageConverter;
import org.springframework.web.client.RestTemplate;
import java.nio.charset.Charset;

public class RestTemplateUtil {
    /**
     * 创建指定字符集的RestTemplate
     *
     * @param charset
     * @return
     */
    public static RestTemplate getInstance(String charset) {
        //复杂构造函数的使用
        SimpleClientHttpRequestFactory requestFactory = new SimpleClientHttpRequestFactory();
        requestFactory.setConnectTimeout(30000);// 设置超时
        requestFactory.setReadTimeout(30000);
        RestTemplate restTemplate = new RestTemplate();
        restTemplate.setRequestFactory(requestFactory);
        restTemplate.getMessageConverters().add(new StringHttpMessageConverter(Charset.forName(charset)));
        return restTemplate;
    }

}
```
2、准备用于封装json数据的对象实体，封装完成后可用JSONObject转为json自查参数，在发送请求时不需要传入json，使用实体类作为参数即可。
```java
import com.alibaba.fastjson.JSONObject;

/**
 * 实体类
 */
public class DataJson{
    private int id;
}

/**
 * 查看json参数
 */
class test{
    System.out.println(JSONObject.toJSONString(new DataJson));
}
```
3、发送post请求，并处理返回结果
```java
class test{
  /**
   * 发送http请求
   * @param dataJson
   * @return
   */
  public String testJavaObj(EndDataJson dataJson) {
    RestTemplate restTemplate = RestTemplateUtil.getInstance("utf-8");
    String url = "你所请求的接口完整地址";
    //设置header信息
    HttpHeaders requestHeaders = new HttpHeaders();
    requestHeaders.setContentType(MediaType.APPLICATION_JSON);
    //根据接口文档添加对应值，这里以Content-Type为例
    requestHeaders.add("Content-Type","application/json;charset=utf-8");
    //将头和body放进HttpEntity
    HttpEntity requestEntity =new HttpEntity(dataJson,requestHeaders);
    //发送请求，返回值为json，这里用字符串接收
    String result = restTemplate.postForObject(url,requestEntity, String.class);
    return result;
  }

  /**
   * 处理返回结果
   */
  public void testResult(){
    String result = testJavaObj(dataJson);
    //转为json对象体
    JSONObject jsonObject = JSONObject.parseObject(result);
    //以提取code值为例
    String code = jsonObject.getString("code");
    //当key对应的是对象时
    JSONObject ifResultInfo = jsonObject.getJSONObject("key");
    ifResultInfo.getString("key");
  }
}
```
