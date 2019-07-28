---
title: JAVA inputStream源码简介（jdk1.8）
date: 2019-03-22 00:16:55
tags:
    - JAVA
---

### InputStream源码简述

> 这个抽象类是表示输入字节流的所有类的超类。

<!--more-->

```java
public abstract class InputStream implements Closeable{
    private static final int MAX_SKIP_BUFFER_SIZE = 2048;//
    //从输入流读取数据的下一个字节，值字节作为int范围0至255，如果没有字节可用，因为已经到达流的末尾，所以返回-1.该方法阻塞直到输入数据可用，检测到流的结尾，或抛出异常
    //强制所有子类实现
    public abstract int read() throws IOException;
   //从输出流读取一些字节数存入缓冲区b，返回实际读取的字节数
    public int read(byte b[]) throws IOException {
        return read(b, 0, b.length);
    }
    //从输入流读取len长度的字节数组，尝试读取多达len个字节，但可以读取较小的数字，实际读取的字节数作为整数返回
    public int read(byte b[], int off, int len) throws IOException {
        //判断边界条件
        if (b == null) {
            throw new NullPointerException();
        } else if (off < 0 || len < 0 || len > b.length - off) {
            throw new IndexOutOfBoundsException();
        } else if (len == 0) {
            return 0;
        }
		//读取一个字节先
        int c = read();
        if (c == -1) {
            return -1;
        }
        b[off] = (byte)c;
		
        int i = 1;
        try {
            //读取len长度的数
            for (; i < len ; i++) {
                c = read();
                if (c == -1) {
                    break;
                }
                b[off + i] = (byte)c;
            }
        } catch (IOException ee) {
        }
        //返回实际读取字节数
        return i;
    }
  public long skip(long n) throws IOException {

        long remaining = n;	
        int nr;
		//n为负，返回跳过字节数为0个
        if (n <= 0) {
            return 0;
        }
		//是否超出最大缓冲区长度
        int size = (int)Math.min(MAX_SKIP_BUFFER_SIZE, remaining);
        byte[] skipBuffer = new byte[size];
      //read读取要丢弃的字节数
        while (remaining > 0) {
            nr = read(skipBuffer, 0, (int)Math.min(size, remaining));
            if (nr < 0) {
                break;
            }
            remaining -= nr;
        }
		//返回实际跳过的字节数
        return n - remaining;
    }
    //返回从该输入流可以读取或跳过的字节数的估计值，而不会被下一次调用此输入流的方法阻塞
  	public int available() throws IOException {
        return 0;
    }
 	 //来自closeable接口
  	public void close() throws IOException {}
  	  //标记此输入流中的当前位置，readlimit参数告诉这个输入流，以允许在标记位置无效之前读取许多字节。 
  	public synchronized void mark(int readlimit) {}
   	//流重新定位到上次在此输入流上调用mark方法时的位置。 
  	public synchronized void reset() throws IOException {
        throw new IOException("mark/reset not supported");
    }
    //返回是否支持mark和reset方法
    public boolean markSupported() {
        return false;
    }
}
```

**抽象类InputStream的直接子类：**

​	*AudioInputStream、ByteArrayInputStream、FileInputStream、FilterInputStream、 InputStream、ObjectInputStream、PipedInputStream、SequenceInputStream、StringBufferInputStream* 

