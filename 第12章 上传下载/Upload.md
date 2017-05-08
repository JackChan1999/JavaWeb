[HTTP POST请求报文格式分析与Java实现文件上传](http://blog.csdn.net/bboyfeiyu/article/details/41863951)

在开发中，我们使用的比较多的HTTP请求方式基本上就是GET、POST。其中GET用于从服务器获取数据，POST主要用于向服务器提交一些表单数据，例如文件上传等。而我们在使用HTTP请求时中遇到的比较麻烦的事情就是构造文件上传的HTTP报文格式，这个格式虽说也比较简单，但也比较容易出错。今天我们就一起来学习HTTP POST的报文格式以及通过Java来模拟文件上传的请求。

首先我们来看一个POST的报文请求，然后我们再来详细的分析它。

## POST报文格式

```
POST /api/feed/ HTTP/1.1  
Accept-Encoding: gzip  
Content-Length: 225873  
Content-Type: multipart/form-data; boundary=OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp  
Host: www.myhost.com  
Connection: Keep-Alive  

--OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp  
Content-Disposition: form-data; name="lng"  
Content-Type: text/plain; charset=UTF-8  
Content-Transfer-Encoding: 8bit  

116.361545  
--OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp  
Content-Disposition: form-data; name="lat"  
Content-Type: text/plain; charset=UTF-8  
Content-Transfer-Encoding: 8bit  

39.979006  
--OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp  
Content-Disposition: form-data; name="images"; filename="/storage/emulated/0/Camera/jdimage/1xh0e3yyfmpr2e35tdowbavrx.jpg"  
Content-Type: application/octet-stream  
Content-Transfer-Encoding: binary  

这里是图片的二进制数据  
--OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp--  
```

这里我们提交的是经度、纬度和一张图片（图片数据比较长，而且比较杂乱，这里省略掉了）。

## 格式分析

### 请求头分析

我们先看报文格式中的第一行：
```
POST /api/feed/ HTTP/1.1  
```
这一行就说明了这个请求的请求方式，即为POST方式，要请求的子路径为/api/feed/,例如我们的服务器地址为`www.myhost.com`，然后我们的这个请求的完整路径就是`www.myhost.com/api/feed/`，最后说明了HTTP协议的版本号为1.1
```
Accept-Encoding: gzip  
Content-Length: 225873  
Content-Type: multipart/form-data; boundary=OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp  
Host: www.myhost.com  
Connection: Keep-Alive
```
这几个header的意思分别为服务器返回的数据需要使用gzip压缩、请求的内容长度为225873、内容的类型为"multipart/form-data"、请求参数分隔符(boundary)为OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp、请求的根域名为`www.myhost.com`、HTTP连接方式为持久连接( Keep-Alive)。

其中这里需要注意的一点是分隔符，即boundary。boundary用于作为请求参数之间的界限标识，例如参数1和参数2之间需要有一个明确的界限，这样服务器才能正确的解析到参数1和参数2。但是分隔符并不仅仅是boundary，而是下面这样的格式：-- + boundary。例如这里的boundary为OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp，那么参数分隔符则为:
```
--OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp
```
不管boundary本身有没有这个"--"，这个"--"都是不能省略的。

我们知道HTTP协议采用“请求-应答”模式，当使用普通模式，即非KeepAlive模式时，每个请求/应答客户和服务器都要新建一个连接，完成之后立即断开连接（HTTP协议为无连接的协议）；当使用Keep-Alive模式（又称持久连接、连接重用）时，Keep-Alive功能使客户端到服务器端的连接持续有效，当出现对服务器的后续请求时，Keep-Alive功能避免了建立或者重新建立连接。

![](img/upload.png)

如上图中，左边的是关闭Keep-Alive的情况，每次请求都需要建立连接，然后关闭连接；右边的则是Keep-Alive，在第一次建立请求之后保持连接，然后后续的就不需要每次都建立、关闭连接了，启用Keep-Alive模式肯定更高效，性能更高，因为避免了建立/释放连接的开销。

http 1.0中默认是关闭的，需要在http头加入"Connection: Keep-Alive"，才能启用Keep-Alive；http 1.1中默认启用Keep-Alive，如果加入"Connection: close "，才关闭。目前大部分浏览器都是用http1.1协议，也就是说默认都会发起Keep-Alive的连接请求了，所以是否能完成一个完整的Keep- Alive连接就看服务器设置情况。

### 请求实体分析

请求实体其实就是HTTP POST请求的参数列表，每个参数以请求分隔符开始，即-- + boundary。例如下面这个参数。
```
--OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp  
Content-Disposition: form-data; name="lng"  
Content-Type: text/plain; charset=UTF-8  
Content-Transfer-Encoding: 8bit  

116.361545
```
上面第一行为--OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp，也就是--加上boundary内容，最后加上一个换行 （这个换行不能省略），换行的字符串表示为"\r\n"。第二行为Content-Disposition和参数名,这里的参数名为lng，即经度。Content-Disposition就是当用户想把请求所得的内容存为一个文件的时候提供一个默认的文件名，这里我们不过多关注。第三行为Content-Type，即WEB 服务器告诉浏览器自己响应的对象的类型，还有指定字符编码为UTF-8。第四行是描述的是消息请求(request)和响应(response)所附带的实体对象(entity)的传输形式，简单文本数据我们设置为8bit，文件参数我们设置为binary就行。然后添加两个换行之后才是参数的具体内容。例如这里的参数内容为116.361545。

注意这里的每行之间都是使用“\r\n”来换行的，最后一行和参数内容之间是两个换行。文件参数也是一样的格式，只是文件参数的内容是字节流。

这里要注意一下，普通文本参数和文件参数有如下两个地方的不同，因为其内容本身的格式是不一样的。

普通参数：

```
Content-Type: text/plain; charset=UTF-8  
Content-Transfer-Encoding: 8bit  
```
文件参数：

```
Content-Type: application/octet-stream  
Content-Transfer-Encoding: binary  
```
参数实体的最后一行是: --加上boundary加上--，最后换行，这里的 格式即为: --OCqxMF6-JxtxoMDHmoG5W5eY9MGRsTBp--

## 模拟文件上传请求

```java
    public static void uploadFile(String fileName) {  
        try {  

            // 换行符  
            final String newLine = "\r\n";  
            final String boundaryPrefix = "--";  
            // 定义数据分隔线  
            String BOUNDARY = "========7d4a6d158c9";  
            // 服务器的域名  
            URL url = new URL("www.myhost.com");  
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();  
            // 设置为POST情  
            conn.setRequestMethod("POST");  
            // 发送POST请求必须设置如下两行  
            conn.setDoOutput(true);  
            conn.setDoInput(true);  
            conn.setUseCaches(false);  
            // 设置请求头参数  
            conn.setRequestProperty("connection", "Keep-Alive");  
            conn.setRequestProperty("Charsert", "UTF-8");  
            conn.setRequestProperty("Content-Type", "multipart/form-data; boundary=" + BOUNDARY);  

            OutputStream out = new DataOutputStream(conn.getOutputStream());  

            // 上传文件  
            File file = new File(fileName);  
            StringBuilder sb = new StringBuilder();  
            sb.append(boundaryPrefix);  
            sb.append(BOUNDARY);  
            sb.append(newLine);  
            // 文件参数,photo参数名可以随意修改  
            sb.append("Content-Disposition: form-data;name=\"photo\";filename=\"" + fileName  
                    + "\"" + newLine);  
            sb.append("Content-Type:application/octet-stream");  
            // 参数头设置完以后需要两个换行，然后才是参数内容  
            sb.append(newLine);  
            sb.append(newLine);  

            // 将参数头的数据写入到输出流中  
            out.write(sb.toString().getBytes());  

            // 数据输入流,用于读取文件数据  
            DataInputStream in = new DataInputStream(new FileInputStream(  
                    file));  
            byte[] bufferOut = new byte[1024];  
            int bytes = 0;  
            // 每次读1KB数据,并且将文件数据写入到输出流中  
            while ((bytes = in.read(bufferOut)) != -1) {  
                out.write(bufferOut, 0, bytes);  
            }  
            // 最后添加换行  
            out.write(newLine.getBytes());  
            in.close();  

            // 定义最后数据分隔线，即--加上BOUNDARY再加上--。  
            byte[] end_data = (newLine + boundaryPrefix + BOUNDARY + boundaryPrefix + newLine)  
                    .getBytes();  
            // 写上结尾标识  
            out.write(end_data);  
            out.flush();  
            out.close();  

            // 定义BufferedReader输入流来读取URL的响应  
//            BufferedReader reader = new BufferedReader(new InputStreamReader(  
//                    conn.getInputStream()));  
//            String line = null;  
//            while ((line = reader.readLine()) != null) {  
//                System.out.println(line);  
//            }  

        } catch (Exception e) {  
            System.out.println("发送POST请求出现异常！" + e);  
            e.printStackTrace();  
        }  
    }  
```
## 使用Apache Httpmime上传文件

```java
/**
 * @param fileName 图片路径
 */  
public static void uploadFileWithHttpMime(String fileName) {  

    // 定义请求url  
    String uri = "www.myhost.com";  

    // 实例化http客户端  
    HttpClient httpClient = new DefaultHttpClient();  
    // 实例化post提交方式  
    HttpPost post = new HttpPost(uri);  

    // 添加json参数  
    try {  
        // 实例化参数对象  
        MultipartEntity params = new MultipartEntity();  

        // 图片文本参数  
        params.addPart("textParams", new StringBody(  
                "{'user_name':'我的用户名','channel_name':'却道明','channel_address':'(123.4,30.6)'}",  
                Charset.forName("UTF-8")));  

        // 设置上传文件  
        File file = new File(fileName);  
        // 文件参数内容  
        FileBody fileBody = new FileBody(file);  
        // 添加文件参数  
        params.addPart("photo", fileBody);  
        params.addPart("photoName", new StringBody(file.getName()));  
        // 将参数加入post请求体中  
        post.setEntity(params);  

        // 执行post请求并得到返回对象 [ 到这一步我们的请求就开始了 ]  
        HttpResponse resp = httpClient.execute(post);  

        // 解析返回请求结果  
        HttpEntity entity = resp.getEntity();  
        InputStream is = entity.getContent();  
        BufferedReader reader = new BufferedReader(new InputStreamReader(is));  
        StringBuffer buffer = new StringBuffer();  
        String temp;  

        while ((temp = reader.readLine()) != null) {  
            buffer.append(temp);  
        }  

        System.out.println(buffer);  

    } catch (UnsupportedEncodingException e) {  
        e.printStackTrace();  
    } catch (ClientProtocolException e) {  
        e.printStackTrace();  
    } catch (IOException e) {  
        e.printStackTrace();  
    } catch (IllegalStateException e) {  
        e.printStackTrace();  
    }  

}
```
[HttpMime.jar下载地址](http://hc.apache.org/downloads.cgi)，下载httpClient的压缩包即可，httpmime.jar包含在其中。

## 手动实现上传文件

```java
/*
 * The MIT License (MIT)
 *
 * Copyright (c) 2014-2015 JackChan, Inc
 *
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the "Software"), to deal
 * in the Software without restriction, including without limitation the rights
 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 * copies of the Software, and to permit persons to whom the Software is
 * furnished to do so, subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in
 * all copies or substantial portions of the Software.
 * 
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 * THE SOFTWARE.
 */

package org.simple.net.entity;

import android.text.TextUtils;

import org.apache.http.Header;
import org.apache.http.HttpEntity;
import org.apache.http.message.BasicHeader;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.Closeable;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.Random;

/**
 * POST报文格式请参考博客 : http://blog.csdn.net/bboyfeiyu/article/details/41863951.
 * <p>
 * Android中的多参数类型的Entity实体类,用户可以使用该类来上传文件、文本参数、二进制参数,
 * 不需要依赖于httpmime.jar来实现上传文件的功能.
 * </p>
 * 
 */
public class MultipartEntity implements HttpEntity {

    private final static char[] MULTIPART_CHARS = "-_1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
            .toCharArray();
    /**
     * 换行符
     */
    private final String NEW_LINE_STR = "\r\n";
    private final String CONTENT_TYPE = "Content-Type: ";
    private final String CONTENT_DISPOSITION = "Content-Disposition: ";
    /**
     * 文本参数和字符集
     */
    private final String TYPE_TEXT_CHARSET = "text/plain; charset=UTF-8";

    /**
     * 字节流参数
     */
    private final String TYPE_OCTET_STREAM = "application/octet-stream";
    /**
     * 二进制参数
     */
    private final byte[] BINARY_ENCODING = "Content-Transfer-Encoding: binary\r\n\r\n".getBytes();
    /**
     * 文本参数
     */
    private final byte[] BIT_ENCODING = "Content-Transfer-Encoding: 8bit\r\n\r\n".getBytes();

    /**
     * 分隔符
     */
    private String mBoundary = null;
    /**
     * 输出流
     */
    ByteArrayOutputStream mOutputStream = new ByteArrayOutputStream();

    public MultipartEntity() {
        this.mBoundary = generateBoundary();
    }

    /**
     * 生成分隔符
     * 
     * @return
     */
    private final String generateBoundary() {
        final StringBuffer buf = new StringBuffer();
        final Random rand = new Random();
        for (int i = 0; i < 30; i++) {
            buf.append(MULTIPART_CHARS[rand.nextInt(MULTIPART_CHARS.length)]);
        }
        return buf.toString();
    }

    /**
     * 参数开头的分隔符
     * 
     * @throws IOException
     */
    private void writeFirstBoundary() throws IOException {
        mOutputStream.write(("--" + mBoundary + "\r\n").getBytes());
    }

    /**
     * 添加文本参数
     * 
     * @param key
     * @param value
     */
    public void addStringPart(final String paramName, final String value) {
        writeToOutputStream(paramName, value.getBytes(), TYPE_TEXT_CHARSET, BIT_ENCODING, "");
    }

    /**
     * 将数据写入到输出流中
     * 
     * @param key
     * @param rawData
     * @param type
     * @param encodingBytes
     * @param fileName
     */
    private void writeToOutputStream(String paramName, byte[] rawData, String type,
            byte[] encodingBytes,
            String fileName) {
        try {
            writeFirstBoundary();
            mOutputStream.write((CONTENT_TYPE + type + NEW_LINE_STR).getBytes());
            mOutputStream
                    .write(getContentDispositionBytes(paramName, fileName));
            mOutputStream.write(encodingBytes);
            mOutputStream.write(rawData);
            mOutputStream.write(NEW_LINE_STR.getBytes());
        } catch (final IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 添加二进制参数, 例如Bitmap的字节流参数
     * 
     * @param key
     * @param rawData
     */
    public void addBinaryPart(String paramName, final byte[] rawData) {
        writeToOutputStream(paramName, rawData, TYPE_OCTET_STREAM, BINARY_ENCODING, "no-file");
    }

    /**
     * 添加文件参数,可以实现文件上传功能
     * 
     * @param key
     * @param file
     */
    public void addFilePart(final String key, final File file) {
        InputStream fin = null;
        try {
            fin = new FileInputStream(file);
            writeFirstBoundary();
            final String type = CONTENT_TYPE + TYPE_OCTET_STREAM + NEW_LINE_STR;
            mOutputStream.write(getContentDispositionBytes(key, file.getName()));
            mOutputStream.write(type.getBytes());
            mOutputStream.write(BINARY_ENCODING);

            final byte[] tmp = new byte[4096];
            int len = 0;
            while ((len = fin.read(tmp)) != -1) {
                mOutputStream.write(tmp, 0, len);
            }
            mOutputStream.flush();
        } catch (final IOException e) {
            e.printStackTrace();
        } finally {
            closeSilently(fin);
        }
    }

    private void closeSilently(Closeable closeable) {
        try {
            if (closeable != null) {
                closeable.close();
            }
        } catch (final IOException e) {
            e.printStackTrace();
        }
    }

    private byte[] getContentDispositionBytes(String paramName, String fileName) {
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append(CONTENT_DISPOSITION + "form-data; name=\"" + paramName + "\"");
        // 文本参数没有filename参数,设置为空即可
        if (!TextUtils.isEmpty(fileName)) {
            stringBuilder.append("; filename=\""
                    + fileName + "\"");
        }

        return stringBuilder.append(NEW_LINE_STR).toString().getBytes();
    }

    @Override
    public long getContentLength() {
        return mOutputStream.toByteArray().length;
    }

    @Override
    public Header getContentType() {
        return new BasicHeader("Content-Type", "multipart/form-data; boundary=" + mBoundary);
    }

    @Override
    public boolean isChunked() {
        return false;
    }

    @Override
    public boolean isRepeatable() {
        return false;
    }

    @Override
    public boolean isStreaming() {
        return false;
    }

    @Override
    public void writeTo(final OutputStream outstream) throws IOException {
        // 参数最末尾的结束符
        final String endString = "--" + mBoundary + "--\r\n";
        // 写入结束符
        mOutputStream.write(endString.getBytes());
        //
        outstream.write(mOutputStream.toByteArray());
    }

    @Override
    public Header getContentEncoding() {
        return null;
    }

    @Override
    public void consumeContent() throws IOException,
            UnsupportedOperationException {
        if (isStreaming()) {
            throw new UnsupportedOperationException(
                    "Streaming entity does not implement #consumeContent()");
        }
    }

    @Override
    public InputStream getContent() {
        return new ByteArrayInputStream(mOutputStream.toByteArray());
    }
}
```
```java
/**
     * 发送MultipartRequest,可以传字符串参数、文件、Bitmap等参数,这种请求为POST类型
     */
    protected void sendMultiRequest() {
        // 2、创建请求
        MultipartRequest multipartRequest = new MultipartRequest("你的url",
                new RequestListener<String>() {
                    @Override
                    public void onComplete(int stCode, String response, String errMsg) {
                        // 该方法执行在UI线程
                    }
                });

        // 3、添加各种参数
        // 添加header
        multipartRequest.addHeader("header-name", "value");

        // 通过MultipartEntity来设置参数
        MultipartEntity multi = multipartRequest.getMultiPartEntity();
        // 文本参数
        multi.addStringPart("location", "模拟的地理位置");
        multi.addStringPart("type", "0");

        Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.ic_launcher);
        // 直接从上传Bitmap
        multi.addBinaryPart("images", bitmapToBytes(bitmap));
        // 上传文件
        multi.addFilePart("imgfile", new File("storage/emulated/0/test.jpg"));

        // 4、将请求添加到队列中
        mQueue.addRequest(multipartRequest);
    }

    private byte[] bitmapToBytes(Bitmap bitmap) {
        ByteArrayOutputStream stream = new ByteArrayOutputStream();
        bitmap.compress(Bitmap.CompressFormat.PNG, 100, stream);
        return stream.toByteArray();
    }
```