# 网络编程

##  1.1 概述

计算机网络

网络编程的目的

1. 如何定位网络上一台主机：192.168.68.13：端口，定位到计算机上某个资源

2. 找到主机，传输数据

javaweb：网页编程  B/S

网络编程：TCP/IP     C/S

## 1.2 网络通信的要素

如何实现网络的通信

通信双方地址：

* ip
* 端口号

**规则：网络通信的协议**

TCP/IP参考模型: 主要学习UDP和TCP协议

![TCP/IP参考模型](https://img-blog.csdnimg.cn/2019042317573274.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzI3MjEwMA==,size_16,color_FFFFFF,t_70)

![img](https://images2015.cnblogs.com/blog/750327/201608/750327-20160822164857776-669486844.gif)

## 1.3 IP

ip地址：InetAddress

* 唯一定位一台网络上的计算机
* 127.0.0.1： 本机 localhost
* ip地址分类
  * ip地址分类
    * ipv4 / ipv6
      * ipv4 127.0.0.1 4个字节组成 总共大约42亿
      * ipv6  128位 8个无符号整数 16进制
  * 公网（互联网）-私网（局域网）
    * 192.168.xx.xx 专门给组织内部使用
    * ABCD类地址
* 域名：记忆的问题
  * IP www.jd.com

## 1.4 端口

端口表示程序的进程

* 不同的进程有不同的端口号，用来区分软件

* 被规定 0~65535

* TCP，UDP端口: 65535*2  单个协议下端口不能冲突

* 端口分类：

  * 公有端口 0~1023

    * http： 80
    * https：43
    * FTP：21
    * telent：23

  * 程序注册端口：1024~49151.分配用户或者程序

    * tomcat 8080
    * MySQL 3306
    * Oracle 1521

  * 动态、私有端口：49152~65535

    ```bash
    netstat -ano   # 查看所有端口
    netstat -ano|findstr "5900" # 查看指定的端口
    tasklist|findstr "8696" # 查看指定端口进程
    ```

```java
package com.lessonIP;

import java.net.InetSocketAddress;

public class TestInetSocketAddress {
    public static void main(String[] args) {
        InetSocketAddress inetSocketAddress = new InetSocketAddress("127.0.0.1", 8080);
        InetSocketAddress inetSocketAddress2 = new InetSocketAddress("localhost", 8080);


        System.out.println(inetSocketAddress.getAddress());
        System.out.println(inetSocketAddress.getHostName()); //地址
        System.out.println(inetSocketAddress.getPort()); //端口
    }
}
```

## 1.5 通信协议

网络通信协议：速率，传输码率，代码结构，传输控制

问题：非常的复杂

大事化小：分层

**TCP/IP 协议簇**

重要：

* TCP：用户传输协议
* UDP：用户数据报协议

出名的协议：

* TCP：
* IP：网络互联协议

### TCP UDP 对比

TCP：

* 类似于打电话

* 连接稳定

* 三次握手，四次挥手

  ```
  最少需要3次，保证稳定连接
  A：你瞅啥
  B：瞅你咋地
  A：干一场
  
  A：我要走了！
  B：你要走了嘛？
  B：你真的要走了嘛？
  A：我真的要走了！
  ```

* 客户端、服务端

* 传输完成，释放连接，效率低

UDP：

* 发短信
* 不连接，不稳定
* 随时可以发
* DDOS：洪水攻击，饱和攻击

## 1.6 TCP实现聊天

客户端：

1. 连接服务器，Socket
2. 发送消息

服务器

1. 建立服务的端口, ServerSocket
2. 等待用户的链接 accept
3. 接受用户的消息

```java
package com.lessonIP.lesson01;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;
import java.net.UnknownHostException;
import java.nio.charset.StandardCharsets;

//客户端
public class TcpClientDemo01 {
    public static void main(String[] args) {

        Socket socket = null;
        OutputStream os = null;
        try {
            //1.要知道服务器的地址 端口号
            InetAddress serverIp = InetAddress.getByName("127.0.0.1");
            int port = 9999;
            //3. 创建一个socket连接
            socket = new Socket(serverIp, port);
            //3. 发送消息 IO流
            os = socket.getOutputStream();
            os.write("hello".getBytes());
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //关闭资源
            if (os!=null){
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            //关闭资源
            if (socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

```java
package com.lessonIP.lesson01;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

//服务端
public class TcpServerDemo01 {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        ByteArrayOutputStream baos = null;
        try {
            //1. 我得有个地址
            serverSocket = new ServerSocket(9999);
            //2. 等待客户端连接
            socket = serverSocket.accept();
            //3. 读取客户端的消息
            is = socket.getInputStream();
            
            //管道流
            baos = new ByteArrayOutputStream();
            byte[] buffer = new byte[1024];
            int len;
            while ((len = is.read(buffer))!=-1){
                baos.write(buffer,0, len);
            }
            System.out.println(baos.toString());

        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //关闭资源
            if (baos!=null){
                try {
                    baos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (serverSocket!=null){
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 1.7 TCP实现文件上传

```java
package com.lessonIP.lesson02;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TcpServerDemo02 {
    public static void main(String[] args) throws IOException {
        //1. 创建服务
        ServerSocket serverSocket = new ServerSocket(9000);
        //2.监听客户端的连接
        Socket socket = serverSocket.accept();//阻塞式监听，会一直等待客户端连接
        //3、获取输入流
        InputStream is = socket.getInputStream();
        //4、文件输出
        FileOutputStream fos = new FileOutputStream(new File("receive.txt"));

        //4 写出文件
        byte[] buffer = new byte[1024];
        int len;
        while((len=is.read(buffer))!=-1){
            fos.write(buffer,0,len);
        }
        //通知服务器我已经传输完了
        socket.shutdownOutput();

        //通知客户端我接收完毕
        OutputStream os = socket.getOutputStream();
        os.write("接受完毕".getBytes());

        //关闭资源
        fos.close();
        is.close();
        socket.close();
    }
}
```

```java
package com.lessonIP.lesson02;

import java.io.*;
import java.net.InetAddress;
import java.net.Socket;

public class TcpClientDemo02 {

    public static void main(String[] args) throws IOException {
        //1、创建socket连接
        Socket socket = new Socket(InetAddress.getLocalHost(), 9000);
        //2. 创建输出流
        OutputStream os = socket.getOutputStream();
        //3 文件流
        FileInputStream fis = new FileInputStream(new File("F:\\IdeaProjects\\javaSE\\基础语法\\test.txt"));
        //4 写出文件
        byte[] buffer = new byte[1024];
        int len;
        while((len=fis.read(buffer))!=-1){
            os.write(buffer,0,len);
        }

        //确定服务器接收到了，才能够断开连接
        InputStream is = socket.getInputStream();
        ByteArrayOutputStream baos= new ByteArrayOutputStream();
        byte[] buffer2 = new byte[1024];
        int len2;
        while((len2=is.read(buffer2))!=-1){
            baos.write(buffer2,0,len2);
        }

        System.out.println(baos.toString());

        //5. 关闭资源
        baos.close();
        is.close();
        fis.close();
        os.close();
        socket.close();
    }

}
```

#### 初识 Tomcat

服务端

* 自定义 S
* Tomcat服务器 S：java后台开发

客户端

* 自定义 C
* 浏览器 B

## 1.8 UDP

发短信：不用链接，直接发送

接收端

```java
package com.lessonIP.lesson03;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

//还是要等待客户的链接
public class UdpServerDemo01 {
    public static void main(String[] args) throws Exception {
        //开放端口
        DatagramSocket socket = new DatagramSocket(9090);
        //接受数据包
        byte[] buffer = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length);

        socket.receive(packet);//阻塞 接收

        System.out.println(packet.getAddress().getHostAddress());
        System.out.println(new String(packet.getData(),0, packet.getLength()));
        //关闭连接
        socket.close();
    }
}
```

发送端

```java
package com.lessonIP.lesson03;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UdpClientDemo01 {
    public static void main(String[] args) throws Exception {
        //1. 建立一个Socket
        DatagramSocket socket = new DatagramSocket();

        //2. 建个包
        String msg = "你好啊";
        //发送给谁，服务器的地址
        InetAddress localhost = InetAddress.getByName("localhost");
        int port = 9090;
        DatagramPacket packet = new DatagramPacket(
                msg.getBytes(),
                0,
                msg.getBytes().length,
                localhost,
                port
        );

        //3. 发送包
        socket.send(packet);

        //关闭流
        socket.close();

    }
}
```

#### 在线咨询

接受线程

```java
package com.lessonIP.chat;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

public class TalkRecvice implements Runnable{
    DatagramSocket socket = null;

    private int fromPort;
    private String msgFrom;

    public TalkRecvice(int fromPort, String msgFrom) {
        this.fromPort = fromPort;
        this.msgFrom = msgFrom;
        try {
            socket = new DatagramSocket(fromPort);
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {

        try {
            // 准备接受包裹
            byte[] container = new byte[1024];
            while (true){
                DatagramPacket packet = new DatagramPacket(container,0,container.length);
                socket.receive(packet);//阻塞式接受消息

                //断开连接
                byte[] data = packet.getData();
                String receiveData = new String(data, 0, packet.getLength());
                System.out.println(msgFrom+":"+receiveData);
                if (receiveData.equals("bye")){
                    break;
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        socket.close();
    }
}
```

发送线程

```java
package com.lessonIP.chat;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetSocketAddress;

public class TalkSend implements Runnable{
    DatagramSocket socket = null;
    BufferedReader reader = null;

    private int fromPort;
    private String toIP;
    private int toPort;

    public TalkSend(int fromPort, String toIP, int toPort) {
        this.fromPort = fromPort;
        this.toIP = toIP;
        this.toPort = toPort;

        try {
            socket = new DatagramSocket(fromPort);
            reader = new BufferedReader(new InputStreamReader(System.in));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {

        while(true){
            String data = null;
            try {
                data = reader.readLine();
                byte[] datas = data.getBytes();
                DatagramPacket packet = new DatagramPacket(
                        datas,
                        0,
                        datas.length,
                        new InetSocketAddress(this.toIP, this.toPort)
                );
                socket.send(packet);
                if (data.equals("bye")){
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        socket.close();
    }
}
```

老师

```java
package com.lessonIP.chat;

public class TalkTeacher {
    public static void main(String[] args) {
        new Thread(new TalkSend(5555,"localhost",8888)).start();
        new Thread(new TalkRecvice(9999, "学生")).start();
    }
}
```

学生

```java
package com.lessonIP.chat;

public class TalkStudent {
    public static void main(String[] args) {
        //开启两个线程
        new Thread(new TalkSend(7777,"localhost",9999)).start();
        new Thread(new TalkRecvice(8888, "老师")).start();
    }
}
```

## 1.9 URL下载网络资源

URL 统一资源定位符

```
1、协议://IP地址:端口号/项目名/资源
```

