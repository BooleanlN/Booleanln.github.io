---
title: ByteArrayInputStream简介源码分析与实例
date: 2019-03-24 22:26:15
tags:
  - java
---

### ByteArrayInputStream

*字节数组输入流，继承于InputStream*

```java
public class ByteArrayInputStream extends InputStream
```

<!--more-->

#### 源码

```java
public
class ByteArrayInputStream extends InputStream {
    protected byte buf[];//字节缓冲数组
    protected int pos;//要读取的字节的索引
    protected int mark = 0;//标记的索引
    protected int count;//字节流长度
    //使用字节数组构造流
    public ByteArrayInputStream(byte buf[]) {
        this.buf = buf;
        this.pos = 0;
        this.count = buf.length;
    }
    //指定范围
    public ByteArrayInputStream(byte buf[], int offset, int length) {
        this.buf = buf;
        this.pos = offset;
        this.count = Math.min(offset + length, buf.length);
        this.mark = offset;
    }
    //读取字节
    //buf[pos++] & 0xff,将byte转换为int型，高位补0
    //byte-8bit,int-32bit
    public synchronized int read() {
        return (pos < count) ? (buf[pos++] & 0xff) : -1;
    }
    //读取指定长度字节，结果保存在b中，返回实际读取字节数
    public synchronized int read(byte b[], int off, int len) {
        if (b == null) {
            throw new NullPointerException();
        } else if (off < 0 || len < 0 || len > b.length - off) {
            throw new IndexOutOfBoundsException();
        }
		//读取结束
        if (pos >= count) {
            return -1;
        }
		//可读取字节数
        int avail = count - pos;
        if (len > avail) {
            len = avail;
        }
        //读取0个字节
        if (len <= 0) {
            return 0;
        }
        //读取结果传输给b[]
        System.arraycopy(buf, pos, b, off, len);
        pos += len;
        return len;
    }
    //跳过n个字节
    public synchronized long skip(long n) {
        long k = count - pos;
        if (n < k) {
            k = n < 0 ? 0 : n;
        }

        pos += k;
        return k;
    }
    //返回剩余可读字节数
	public synchronized int available() {
        return count - pos;
    }
    //是否支持mark
    public boolean markSupported() {
        return true;
    }
    //保存当前位置。readAheadLimit在此处没有任何实际意义
    public void mark(int readAheadLimit) {
        mark = pos;
    }
    //重置到标记位
    public synchronized void reset() {
        pos = mark;
    }
    //关闭流
     public void close() throws IOException {
    }
}
```

#### 分析

- ByteArrayInputStream使用字节数组构造实现
- 实现InputStream的read()函数，用于读取下一字节
- 支持标记功能，可reset()
- avaliable()返回剩余可读取字节数

#### 实例

```java
package IO;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;

/**
 * Created by 13633 on 2019/3/21.
 */
public class ByteArrayISTest {
    public static void main(String[] args) {
        byte[] bytes = new byte[]{0x61,0x62,0x63,0x64,0x65,0x70};
       InputStream inputStream = new ByteArrayInputStream(bytes);
       InputStream inputStream2 = new ByteArrayInputStream(bytes,0,5);
        try {
           while (inputStream.available()!=0){
               int tmp = inputStream.read();//pos++
               System.out.println(Integer.toHexString(tmp));
               if(inputStream.available()==3){
                   inputStream.skip(1);//pos++
                  if(inputStream.markSupported()){
                      inputStream.mark(100);//mark = pos;
                  }
               }
           }
           //reset到mark位置
           inputStream.reset();
           System.out.println(Integer.toHexString(inputStream.read()));
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                inputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

```

