传统的同步，就是按照顺序来，一个任务完成后才能做下一个。

异步，例如Ajax，利用回调函数。这样就可以不用等着一个完成后才做另外一个。

Netty本质是一个NIO框架，基于事件驱动





# I/O模型

## BIO

同步并阻塞。一个连接一个线程。

简单的流程

- 服务器启动一个serversocket
- 客户端启动socket对服务器进行通讯
- 客户端发送请求后，先咨询服务器是否有线程响应，如果无则等待或者被拒绝
- 如果有响应，客户端线程会等待请求结束后，再继续执行

```java
//使用线程池
ExecutorService pool = Executor.newCachedThreadPool();
ServerSocket serverSocket = new ServerSocket(6666);//监听端口
while(true)
{
    final Socket socket = serverSocket.accept();//这里会阻塞
    
    //创建一个线程，与之通讯（单独写一个方法）
    pool.execute(new Runnable(){
        public void run()//重写方法
        {
            handler(socket);
        }
    })；
    
}

public static void handler(Socket socket)
{
    //注意使用try catch
    byte[] bytes = new byte[1024];
    InputStream inputStream = socket.getInputStream();
    while(true)
    {
        int read = inputStream.read(bytes);//这里会阻塞
        if(read!=-1)
        {
            System.out.println(new String(bytes,0,read));
        }
        else
        {
            break;
        }
    }
}
```



使用cmd通讯

- 打开cmd
- telnet ip port 如: telnet 127.0.0.1 6666
- 然后按crtl+]
- 发送消息的指令 send info

## NIO

同步非阻塞。一个线程处理多个请求（连接），即客户端发送的连接请求都会被注册到多路复用器上。

一个thread维护一个selector。selector不断轮询。

核心有三个部分

- channel
- buffer
- selector

selector->channel<--->buffer->client

NIO就是面对缓冲区的编程。

 一个线程从某通道发送请求或者读取数据，但是它仅能获得目前可用的数据，如果目前没有数据可用时，就什么都不会获取，而不是保持线程阻塞，所以直至数据变的可以读取之前，改线程可以继续做其他的事情。非阻塞写也是如此。



### buffer

```java
//buffer
public class test {	

	public static void main(String[] args) {
		//buffer
		//创建buffer
		IntBuffer intBuffer = IntBuffer.allocate(5);
		//存放数据

		for(int i=0;i<intBuffer.capacity();i++)
		{
			intBuffer.put(i*2);
		}

		//将buffer转换 读写切换
		intBuffer.flip();

		while(intBuffer.hasRemaining())
		{
			System.out.println(intBuffer.get());
		}
	}
}

```

### channel

通道可以同时进行读写，实现异步读写数据，可以从缓冲读数据，也可以写数据到缓冲

BIO中的steam是单向的

常用的channel有FileChannel, DatagramChannel, serverSocketChannel

FIleChannel主要是对本地进行IO



流程就是数据->buffer->channel->file

```java
		String str = "hello world";
		//创建一个输出流到channel
		FileOutputStream fileOutputStream =  new FileOutputStream("D:\\Program Files\\vscodeForC++\\file01.txt");

		//通过这个输出流获取文件channel
		//FileChannel真实类型是FileChannelImpl
		FileChannel fileChannel =  fileOutputStream.getChannel();

		//创建一个缓冲区，
		ByteBuffer byteBuffer = ByteBuffer.allocate(1024);

		//将str放入缓冲区
		byteBuffer.put(str.getBytes());

		//对byteBuffer进行反转 ，对position重新定位
		byteBuffer.flip();

		//将bytebuffer写入到channel
		fileChannel.write(byteBuffer);

		fileOutputStream.close();
```







## AIO

并没有广泛应用



## 三大核心原理

- 每个channel都对应一个buffer
- 一个selector对应一个线程
- 一个线程对应多个channel
- channel注册到selector
- 程序切换到哪个channel是由事件决定的 Event
- selector根据不同的事件，在各个通道上切换
- buffer就是一个内存块，底层是一个数组
- 数据的读写是通过buffer
- channel是双向的，可以返回底层系统情况





