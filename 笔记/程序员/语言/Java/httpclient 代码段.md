```java
package com.curender.web.server.http;  
  
import java.io.IOException;  
  
import org.apache.commons.httpclient.DefaultHttpMethodRetryHandler;  
import org.apache.commons.httpclient.Header;  
import org.apache.commons.httpclient.HttpClient;  
import org.apache.commons.httpclient.HttpException;  
import org.apache.commons.httpclient.HttpStatus;  
import org.apache.commons.httpclient.methods.GetMethod;  
import org.apache.commons.httpclient.methods.PostMethod;  
import org.apache.commons.httpclient.methods.PutMethod;  
import org.apache.commons.httpclient.params.HttpMethodParams;  
import org.apache.commons.logging.Log;  
import org.apache.commons.logging.LogFactory;  
  
import com.curender.dal.util.ConstantsDal;  
  
/** 
 * HttpClient GET POST PUT 请求 
 * @author huang 
 * @date 2013-4-10 
 */  
public class HttpRequest  
{  
  
    protected static Log log = LogFactory.getLog(HttpRequest.class);  
    private static HttpRequest httpRequst=null;     
    private HttpRequest(){}    
    public static HttpRequest getInstance(){  
        if(httpRequst==null){  
            synchronized(HttpRequest.class){  
                if(httpRequst == null){  
                    httpRequst=new HttpRequest();  
                }  
            }    
        }  
        return httpRequst;  
    }  
      
    /** 
     * HttpClient GET请求 
     * @author huang 
     * @date 2013-4-9 
     * @param uri 
     * @return resStr 请求返回的JSON数据 
     */  
    public String doGet(String uri){  
        String resStr = null;  
        HttpClient htpClient = new HttpClient();  
        GetMethod getMethod = new GetMethod(ConstantsDal.SERVER_URL+uri);  
        getMethod.getParams().setParameter( HttpMethodParams.RETRY_HANDLER, new DefaultHttpMethodRetryHandler());       
        try{  
            int statusCode = htpClient.executeMethod( getMethod );  
//            log.info(statusCode);  
            if(statusCode != HttpStatus.SC_OK){                
                log.error("Method failed: "+getMethod.getStatusLine());  
                return resStr;  
            }             
            byte[] responseBody = getMethod.getResponseBody();           
            resStr = new String(responseBody,HttpConstants.ENCODED);  
        } catch (HttpException e) {  
            log.error("Please check your provided http address!");  //发生致命的异常，可能是协议不对或者返回的内容有问题  
        } catch (IOException e) {  
            log.error( "Network anomaly" );  //发生网络异常  
        }finally{  
            getMethod.releaseConnection(); //释放连接  
        }  
        return resStr;  
    }  
      
    /** 
     * HttpClient POST请求 
     * @author huang 
     * @date 2013-4-9 
     * @param s_user 
     * @return resStr 请求返回的JSON数据 
     */  
    @SuppressWarnings( "deprecation" )  
    public String doPost(String uri,String jsonObj){      
        String resStr = null;  
        HttpClient htpClient = new HttpClient();  
        PostMethod postMethod = new PostMethod(ConstantsDal.SERVER_URL+uri);  
        postMethod.addRequestHeader( "Content-Type","application/json" );    
        postMethod.getParams().setParameter( HttpMethodParams.HTTP_CONTENT_CHARSET, HttpConstants.ENCODED );  
        postMethod.setRequestBody( jsonObj );  
        try{  
            int statusCode = htpClient.executeMethod( postMethod );  
//            log.info(statusCode);  
            if(statusCode != HttpStatus.SC_OK){  
              //post和put不能自动处理转发          301：永久重定向，告诉客户端以后应从新地址访问    302：Moved Temporarily    
                if(statusCode == HttpStatus.SC_MOVED_PERMANENTLY||statusCode == HttpStatus.SC_MOVED_TEMPORARILY){  
                    Header locationHeader = postMethod.getResponseHeader( "location" );  
                    String location = null;  
                    if(locationHeader!=null){  
                        location = locationHeader.getValue();  
                        log.info("The page was redirected to :"+location);  
                    }else{  
                        log.info("Location field value is null");  
                    }  
                }else{  
                    log.error("Method failed: "+postMethod.getStatusLine());                      
                }  
                return resStr;  
            }                       
            byte[] responseBody = postMethod.getResponseBody();             
            resStr = new String(responseBody,HttpConstants.ENCODED);    
        }catch(Exception e){  
            e.printStackTrace();  
        }finally{  
            postMethod.releaseConnection();  
        }  
        return resStr;  
    }  
      
    /** 
     * HttpClient PUT请求 
     * @author huang 
     * @date 2013-4-10 
     * @return 
     */  
    @SuppressWarnings( "deprecation" )  
    public String doPut(String uri,String jsonObj){  
        String resStr = null;  
        HttpClient htpClient = new HttpClient();  
        PutMethod putMethod = new PutMethod(ConstantsDal.SERVER_URL+uri);  
        putMethod.addRequestHeader( "Content-Type","application/json" );  
        putMethod.getParams().setParameter( HttpMethodParams.HTTP_CONTENT_CHARSET, HttpConstants.ENCODED );  
        putMethod.setRequestBody( jsonObj );  
        try{  
            int statusCode = htpClient.executeMethod( putMethod );  
//            log.info(statusCode);  
            if(statusCode != HttpStatus.SC_OK){  
                log.error("Method failed: "+putMethod.getStatusLine());  
                return null;  
            }    
            byte[] responseBody = putMethod.getResponseBody();           
            resStr = new String(responseBody,HttpConstants.ENCODED);  
        }catch(Exception e){  
            e.printStackTrace();  
        }finally{  
            putMethod.releaseConnection();  
        }  
        return resStr;  
    }  
}  

```