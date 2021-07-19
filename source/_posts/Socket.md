---
title: Socket
date: 2020-10-04 00:00:00
tags: Web
categories: 笔记
description: 一种操作系统提供的进程间通信机制。
---

## Socket简介

Socket（套接字）用于描述IP地址和端口，应用程序可以通过Socket向网络发出请求或者应答网络请求

Socket是支持TCP/IP协议的网络通信的基本操作单元，是对网络通信过程中端点的抽象表示，包含了进行网络通信所必需的5种信息：

连接所使用的协议、本地主机的IP地址、本地进程的协议端口、远地主机的IP地址、远地进程的协议端口



## Socket传输模式

Socket有两种主要的操作方式：**面向连接（TCP）**和**无连接（UDP）**



　　面向连接的Socket操作就像一部电话，Socket必须在发送数据之前与目的地的Socket取得连接，一旦连接建立了，Socket就可以使用一个流接口进行打开、读写以及关闭操作。并且，所有发送的数据在另一端都会以**相同的顺序**被接收。

　　无连接的Socket操作就像一个邮件投递，每一个数据报都是一个独立的单元，它包含了这次投递的所有信息（目的地址和要发送的内容）。在这个模式下的Socket不需要连接目的地Socket，它只是简单的投出数据报。



由此可见：

​		无连接的操作是快速高效的，但是数据安全性不佳；

​		面向连接的操作效率较低，但数据的安全性较好。



## Socket实例

面向连接的Socket主要用到两个类：

服务器端：***ServerSocket*** 客户端：***Socket***



### 第一例：Socket实现客户端向服务器端通信

实现客户端向服务器端的通信只需要启动一个服务器端用来监听客户端发送的消息，然后建立客户端和服务器端的连接，这时客户端想服务器端发送信息就可以接收到了，这也是一个最简单的例子

SocketService（服务器）：

```java
package socket.simple;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;
 
public class SocketService {
    //搭建服务器端
    public static void main(String[] args) throws IOException{
        //创建服务器
        ServerSocket server=new ServerSocket(5209);
        System.out.println("服务器启动成功");
        //等待客户端连接后，接收客户端socket
        Socket socket=server.accept();
        //获取客户端socket的输入流
        BufferedReader in=new BufferedReader(new InputStreamReader(socket.getInputStream()));
        while(true){
            //等待客户端socket的不为空输入流
            String str = in.readline();
            if (str == null) {
                break;
            }
            System.out.println("客户端说：" + str);
        }
        in.close(); //关闭Socket输入流
        socket.close(); //关闭Socket
        server.close(); //关闭ServerSocket
    }
}
```

SocketClient（客户端）：

```java
package socket.simple;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
 
public class SocketClient {
    // 搭建客户端
    public static void main(String[] args) throws IOException {
        //创建客户端，并连接服务器
        Socket socket = new Socket("192.168.10.2", 5209);
        System.out.println("客户端启动成功");
        //获取控制台输入流
        BufferedReader out = new BufferedReader(new InputStreamReader(System.in));
        //通过socket输出流创建write推送功能对象
        PrintWriter pw = new PrintWriter(socket.getOutputStream());
        while (true) {
            //等待控制台不为空的输入流
            String str = out.readline();
            if ("".equals(str)) {
                break;
            }
            //通过socket对象将字符串推送到服务器
            pw.println(str);
            //立刻刷新推送功能对象
            pw.flush();
        } // 继续循环
        pw.close(); // 关闭Socket输出流
        socket.close(); // 关闭Socket
    }
 
}
```

**分析代码**

第一个是阻塞函数，阻塞函数是当这个函数不执行完，函数所在线程就一直停止在这里不动。

上面的例子中，涉及到了两个阻塞函数，一个是***SocketServer.accept()***，一个是***BufferedReader.readline()***。当没有数据读取时，就一直会阻塞在那。也就是说**如果这两个方法获取不到数据的话，程序就停在这了，不再继续往下走了**。

第二个是两个在Socket例子中常用类，***BufferedReader***和***PrintWriter***。

BufferedReader是用来读取输入流的，包括服务器接收客户端推送过来，或者客户端接收服务器推送过来的数据流和控制台输入的数据流。

PrintWriter是用来推送输出流的，可以将数据流从客户端推送给服务器，或者从服务器推送给客户端。

简单的来说，**BufferedReader是接收InputStream；PrintWriter是发送OutputStream**。

它们的实现方式分别是：

```java
BufferedReader br = new BufferedReader(new InputStreamReader(inputStream));

br.readline();



PrintWriter pw = new PrintWriter(outputStream);

pw.println(str);

pw.flush;
```

需要注意的是上面的***br.readline()***和***pw.println(str)***是一起使用的，因为***readline()***读入的数据时必须要有 /r或/n或/r/n ，也就是必须要有换行符，否则会为了等待一个换行、回车符而一直阻塞，所以必须使用***println()***方法。

**分析代码逻辑**

首先我们是先启动服务器，服务器启动后，***accept()***方法会阻塞等待一个socket连接，服务器的代码走到这就停了。

然后我们又启动了客户端。客户端启动后，服务器会往下走，走到循环中的***readline()***方法，阻塞等待获取的客户端socket中输入流的出现，而同时，客户端也会被循环中的***readline()***方法阻塞，等待控制台中的输入流的出现，整个项目就进入了等待中。

这时，我们在客户端的控制台中输入一串字符，客户端中的***readline()***就会立刻读取到这段输入流，然后客户端通过PrintWriter将该输入流通过socket的输出流推送到服务器的socket中，然后进入下一个循环的等待。而服务器中的***readline()***就会立刻读取到socket的输入流，然后打印到服务器的控制台，之后又会进入下一个循环的等待。

理解了逻辑后不难看出，在这个例子中，之所以使用***while(true)***是为了能够一直循环的进入下一轮的输入输出，如果去掉循环，那么就只能实现一次数据的通信。



### 第二例：Socket实现客户端与服务器端通信

在第一个例子中，只是实现了客户端向服务器发送信息，但服务器并没有对客户端进行信息回复。所以第二个例子实现**双向通信**的。为了简化代码，提高可读性，接下来的例子将不再进行资源的关闭回收。

SocketService（服务器）：

```java
package socket.both;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
 
public class SocketService {
    //搭建服务器端
    public static void main(String[] args) throws IOException{
        ServerSocket server=new ServerSocket(5209);
        System.out.println("服务器启动成功");
        Socket socket=server.accept();
        BufferedReader in=new BufferedReader(new InputStreamReader(socket.getInputStream()));
        BufferedReader out=new BufferedReader(new InputStreamReader(System.in));
        PrintWriter pw = new PrintWriter(socket.getOutputStream());
        while(true){
            System.out.println("客户端说："+in.readline());
            String str = out.readline();
            pw.println(str);
            pw.flush();
            System.out.println("服务器说："+str);
        }
    }
}
```

SocketClient（客户端）：

```java
package socket.both;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
 
public class SocketClient {
    // 搭建客户端
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("192.168.10.2", 5209);
        System.out.println("客户端启动成功");
        BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        BufferedReader out = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter pw = new PrintWriter(socket.getOutputStream());
        while (true) {
            String str = out.readline();
            pw.println(str);
            pw.flush();
            System.out.println("客户端说：" + str);
            System.out.println("服务器说："+in.readline());
        }
    }
 
}
```

第二个例子可以非常好的体现出面向连接的Socket发送的数据在另一端会以相同的顺序被接收这个特点。



**分析代码逻辑**

首先我们先启动服务器，然后启动客户端，这时服务器会走到等待客户端发送socket中输入流的步骤（***in.readline()***），客户端会走到等待控制台中输入流的步骤（***out.readline()***）。

然后因为客户端在等待控制台输入，服务器在等待客户端发送信息，所以我们先在客户端的控制台输入一串字符，这时，客户端就会在控制台打印输出这串字符，然后走到等待服务器发送socket中输入流的步骤（***in.readline()***），服务器就会收到这串输入流，然后在控制台打印输出这串输入流，然后走到等待控制台中输入流的步骤（***out.readline()***）。

**之后再服务器端的控制台输入一串字符，又会进入下一个循环的等待。**

这个例子的逻辑是客户端先发送一次通信，服务器接收通信并发送一次通信，客户端再接收并发送一次通信。之前也说了，由于面向连接的Socket发送的数据在另一端会以相同的顺序被接收，所以，如果你在客户端连续发送了两次通信，那么服务器一个循环只会接收一次通信，然后服务器再次发送一次通信后，才会接收到客户端的第二次通信。理由是下面的***readline()***已经读到数据可以继续执行了，但上面的***readline()***还没有读到数据，程序在这就已经阻塞了。

**这个特点会导致一个现象，就是客户端发送几十几百次消息，但服务器只接收到了第一个消息。其实是剩下的消息还没有进行读取而已。**

以上两个例子是为了帮助更好的理解Socket应该怎么使用，但是只是一对一的通信，而且是服务器对客户端的一对一通信，那么多个客户端怎么连接服务器呢？客户端和客户端之间怎么通信呢？这就是Socket的聊天室功能了。



### 第三例：Socket实现多个客户端向服务器端通信

​		实现多个客户端向服务器端的通信首先需要启动一个服务器端用来监听客户端的连接，然后会将连接放入线程中，每当有客户端向服务器请求连接时，服务器会获取客户端的Socket，然后将这个Socket放入一个新的线程中，在线程中监听客户端发送的信息，这时客户端向服务器端发送信息就可以接收到了。



SocketService（服务器）：

```java
package socket.service;
 
import java.net.ServerSocket;
import java.net.Socket;
 
public class SocketService {
    public static void main(String args[])throws Exception { 
        ServerSocket serverSocket = new ServerSocket(5208);
        System.out.println("服务器启动成功");
        while (true) {  
            Socket socket= serverSocket.accept();
            System.out.println("上线通知： " + socket.getInetAddress() + ":" +socket.getPort());
            new Thread(new ServerThread(socket)).start();
        }
    }  
}
```



ServerThread（服务器线程）：

```java
package socket.service;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.Socket;
 
public class ServerThread implements Runnable {
    
    public Socket socket;
    
    public ServerThread (Socket socket) {
        this.socket = socket;
    }
 
    @Override
    public void run() {
        try {
            BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            while (true) {
                String str = br.readline();
                System.out.println(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
 
}
```



SocketClient1（第一个客户端）：

```java
package socket.service;
 
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
 
public class SocketClient1 {
    public static void main(String args[])throws Exception{  
        Socket socket = new Socket("192.168.10.2", 5208);  
        System.out.println("小一连接成功");
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter pw = new PrintWriter(socket.getOutputStream());
        while(true){
            pw.println("小一说："+br.readline());
            pw.flush();
        }
    }  
}
```



SocketClient2（第二个客户端）：

```java
package socket.service;
 
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
 
public class SocketClient2 {
    public static void main(String args[])throws Exception{  
        Socket socket = new Socket("192.168.10.2", 5208);  
        System.out.println("小二连接成功");
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter pw = new PrintWriter(socket.getOutputStream());
        while(true){
            pw.println("小二说："+br.readline());
            pw.flush();
        }
    }  
}
```



 　这个例子是多个客户端向服务器发送信息，接下来我们分析一下上面的**代码逻辑**

 　首先我们先启动服务器，服务器启动后，进入无限循环中的***accept()***方法，这里的**无限循环是为了不断的获取n个客户端连接**。在这里会阻塞等待一个socket连接，服务器的代码走到这就停了。

 　然后我们启动第一个客户端。客户端启动后，服务器会往下走，服务器会新启动一个自己写的线程（ServerThread），将socket连接作为构造函数的参数传入这个自己写的线程中，然后在线程中通过***readline()***方法，阻塞等待客户端socket中输入流的出现，而同时，客户端也会被循环中的***readline()***方法阻塞，等待控制台中的输入流的出现。

 　这就是一个客户端的连接，这时整个项目就进入等待中，服务器被***accept()***阻塞等待下一个客户端连接，服务器线程被***readline()***阻塞等待socket中输入流的出现，客户端被***readline()***阻塞等待控制台中输入流的出现。

 　仔细思考后你会发现，**这里的服务器端的功能其实只是实例化出一个个服务器线程，而真正进行通信的是服务器线程和客户端，所以这里的服务器线程其实就是前例的服务器端，而这里的服务器端只是服务器的工厂类，用来生产出一个个服务器的。**

 　之后同样启动第二个客户端，逻辑同第一个客户端连接。然后我们在第一个客户端的控制台中输入一串字符，这时第一个客户端中的***readline()***就会立刻读取到这段输入流，然后客户端通过PrintWriter将该输入流通过socket的输出流推送到服务器线程的socket中，然后进入下一个循环的等待。

 　而服务器线程中的***readline()***就会立刻读取到socket的输入流，然后打印到服务器的控制台，之后又会进入下一个循环的等待。在这里其实也可以进行服务器线程向客户端推送消息的操作，但为了简化代码逻辑，这里没有进行更多的操作，在下一个例子中有详细的解释。

 　然后你在第二个客户端的控制台中输入一串字符，也同样能在服务器端进行打印输出。其实也就是两个客户端可以同时和服务器端的两个线程进行通信，一个服务器端线程阻塞时，不会影响到另一个，所以在服务器端是能够同时看到两个客户端的通信内容的。

 　这里有几个线程的注意事项。

 　第一是自己写线程时一般有两种方式，**继承Thread类**和**实现Runnable接口**，一般情况下，**如果我们只想重写run方法，都是使用实现Runnable接口的方式**，Thread类的底层代码其实也是实现了Runnable接口。

 　第二是启动线程一般也有两种方式，一个是**调用run()方法**，一个是**调用start()方法**，调用run方法其实并没有启动一个新的线程，而是只有一个线程执行了一下run方法。但start是新建了一个线程，所以**线程的启动都是使用start方法**。

 　但实现runnable接口的线程没有start方法，所以需要将实现runnable接口的类封装到Thread类中，然后调用Thread的start方法，如：***new Thread(new ServerThread(socket)).start();***

 　理解了逻辑后不难看出，在这个例子中体现的最重要的一点是利用while(true)循环来实例化多个线程，然后使用一个新的线程来单独跑一个服务器，使得多个客户端能够和多个不同的服务器线程通信，形成了多个客户端同时和一个服务器端通信的效果，如果去掉了循环和线程，那么就只能实例化一个服务器，也就无法让多个socket客户端连接，也就是第一个例子。



### 第四例：Socket实现客户端与客户端通信

​	在上一个例子中，是通过实例化多个服务器线程来和客户端连接，从而实现一个服务器连接多客户端的功能。那么如果我们在服务器线程中获取了客户端推送过来的信息后，再将这个信息推送到所有客户端，是不是就实现了客户端与客户端的通信呢？

 　逻辑是没有错的，但是有两个难点，第一，如何将信息推送到所有客户端呢？所以这就得在服务器端用一个列表来存储所有客户端。第二，我们知道获取控制台的信息需要用到***readline()***这个阻塞函数，获取客户端推送过来的信息也需要用到***readline()***所以每个客户端都应实例化两个线程，一个用来等待控制台输入，一个用来等待服务器推送的消息。

SocketService（服务器）：

```java
package socket.chat;
 
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;
import java.util.List;
 
public class SocketService {
    public static List<Socket> socketList = new ArrayList<Socket>();
    
    public static void main(String args[])throws Exception { 
        ServerSocket serverSocket = new ServerSocket(5208);
        System.out.println("聊天室开启");
        while (true) {  
            Socket socket= serverSocket.accept();     //从连接请求队列中取出一个连接           
            System.out.println("上线通知： 用户" + socket.getPort()+"上线啦！");  
            socketList.add(socket);
            new Thread(new ServerThread(socket)).start();
        }
    }  
}
```

ServerThread（服务器线程）：

```java
package socket.chat;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
 
public class ServerThread implements Runnable {
    
    public Socket socket;
    
    public ServerThread (Socket socket) {
        this.socket = socket;
    }
 
    @Override
    public void run() {
        try {
            BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            while (true) {
                String str = br.readline();
                for (Socket item : SocketService.socketList) {
                    PrintWriter pw = new PrintWriter(item.getOutputStream());
                    pw.println("用户"+socket.getPort()+"说："+str);
                    pw.flush();
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
 
}
```



SocketClient1（第一个客户端）：

```java
package socket.chat;
 
import java.net.Socket;
 
public class SocketClient1 {
    public static void main(String args[])throws Exception{  
        Socket socket = new Socket("192.168.10.2", 5208);
        System.out.println("恭喜你连接成功!");
        new Thread(new SocketThread1(socket)).start();
        new Thread(new SocketThread2(socket)).start();
    }  
}
```



SocketClient2（第二个客户端）：

```java
package socket.chat;
 
import java.net.Socket;
 
public class SocketClient2 {
    public static void main(String args[])throws Exception{  
        Socket socket = new Socket("192.168.10.2", 5208);
        System.out.println("恭喜你连接成功!");
        new Thread(new SocketThread1(socket)).start();
        new Thread(new SocketThread2(socket)).start();
    }
}
```



SocketThread1（监听控制台客户端线程）：

```java
package socket.chat;
 
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
 
public class SocketThread1 implements Runnable {
    
    public Socket socket;
    
    public SocketThread1(Socket socket){
        this.socket = socket;
    }
 
    @Override
    public void run() {
        try {
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            PrintWriter pw = new PrintWriter(socket.getOutputStream());
            while(true){
                String str = br.readline();
                pw.println(str);
                pw.flush();
            }
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } 
        
    }
 
}
```



SocketThread2（监听服务器客户端线程）：

```java
package socket.chat;
 
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.Socket;
 
public class SocketThread2 implements Runnable {
    
    public Socket socket;
    
    public SocketThread2(Socket socket){
        this.socket = socket;
    }
 
    @Override
    public void run() {
        try {
            BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            while(true){
                String str = br.readline();
                System.out.println(str);
            }
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } 
        
    }
 
}
```

 　这个例子是客户端与客户端之间的通信，接下来我们分析一下上面的**代码逻辑**。

 　首先在服务器端定义一个静态的List用来存放所有的客户端socket。我们首先会启动服务器，服务器启动后，进入无限循环中的***accept()***方法，这里的**无限循环是为了不断的获取n个客户端连接**。在这里会阻塞等待一个socket连接，服务器的代码走到这就停了。

 　然后我们启动第一个客户端。客户端启动后，服务器会往下走，首先先将socket连接加入静态列表中，以便服务器线程调用。然后服务器会新启动一个自己写的线程（ServerThread），将socket连接作为构造函数的参数传入这个自己写的线程中，然后在线程中通过***readline()***方法，阻塞等待客户端socket中输入流的出现。

 　这时，客户端也会将socket作为构造函数的参数来新建两个客户端线程，**第一个线程的作用是等待控制台中输入流的出现**，被循环中的***readline()***方法阻塞，等待控制台中的输入流的出现；**第二个线程的作用是等待服务器端推送过来的socket中输入流的出现**。

 　这就是一个客户端的连接，这时整个项目就进入等待中，服务器被***accept()***阻塞等待下一个客户端连接，服务器线程被***readline()***阻塞等待客户端传过来的socket中输入流的出现，客户端的第一个线程被***readline()***阻塞等待控制台中输入流的出现，客户端的第二个线程被***readline()***阻塞等待服务器传过来的socket中输入流的出现。

 　之后同样启动第二个客户端，逻辑同第一个客户端连接。然后我们在第一个客户端的控制台中输入一串字符，这时第一个客户端的第一个线程中的***readline()***就会立刻读取到这段输入流，然后该线程通过***PrintWriter***将该输入流通过socket的输出流推送到服务器线程的socket中，然后进入下一个循环的等待。

 　而服务器线程中的***readline()***就会立刻读取到客户端传过来的socket的输入流，然后通过服务器端的静态列表获取所有的socket连接，再通过每个socket的输出流将这串字符推送到所有客户端，之后又会进入下一个循环的等待。

 　这是所有客户端的第二个线程中的***readline()***就会立刻读取到服务器端传过来的输入流，然后将这串字符串打印输出到控制台，然后进入下一个循环的等待。

 　之后在第二个客户端的控制台中输入一串字符，也是同样的逻辑。上一个例子是多个客户端向服务器的通信，这个例子加了一步服务器接收到通信后再次向所有客户端发送这次通信内容。上一个例子也提到了的服务器可以向客户端通信，这个例子通过**静态List和客户端分两个线程的方式**解决了两个难点，自然而然就实现了聊天室功能。

 　总结一下，本文实现聊天室主要依靠的就是两个技术点：**Socket + 线程**。