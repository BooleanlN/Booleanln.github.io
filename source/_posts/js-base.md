---
title: JS基础
tags:
  - JS
  - 基础语法
url: 160.html
id: 160
categories:
  - JS才是最好的语言
date: 2018-07-16 21:55:37
---

### js组成

*   核心（ECMAScript）：描述了语法、类型、语句、关键字、保留字、运算符、对象等内容，定义了脚本语言的所有属性、方法和对象。
    
*   文档对象模型（DOM）：是HTML和XML的应用程序接口（API）。DOM将把整个页面规划成由节点层级构成的文档。HTML或XML页面的每个部分都是一个节点的衍生物。通过创建树来表示文档，从而使开发者对文档的内容和结构具有空前的控制力。基本统一
    
    *   DOM试图
        
    *   DOM事件
        
    *   DOM样式
        
    *   DOM遍历和范围
        
    *   其他DOM
        
        *   SVG
            
        *   MathML
            
        *   SMTL
            
*   浏览器对象模型（BOM）：对浏览器窗口进行访问和操作。使用BOM，开发者可以移动窗口、改变状态栏中的文本以及执行其他与页面内容不直接相关的动作。不统一
    

### js语法

#### 变量命名规则

*   区分大小写
    
*   变量是弱类型的
    
*   结尾;可有可无
    
*   注释与JAVA、C和PHP相同
    
*   括号表示代码块
    
*   变量第一个字符必须是字母、下划线或美元符号
    
*   余下字母可以是下划线、美元符号或任何字母或数字字符
    
*   Camel标记法
    

#### 关键字

break、else...

#### 保留字

abstract、enum

#### 原始值和引用值

*   原始值：存储在栈中的简单数据段，也就是说它们的值直接存储在变量访问的位置
    
*   typeof()函数返回值：
    
    *   "undefined" 变量是Undefined型：未初始化，即var a;未定义返回也是undefined，函数无返回值也是undefined
        
    *   "boolean" 变量是Boolean型
        
    *   "number" 变量是Number型
        
        *   32位整数，64位浮点数
            
        *   8进制：首字符为0，16进制：首字符为0x，所有数学运算返回结果都为十进制
            
        *   科学记数法：3.125e7
            
        *   Number.MAX\_VALUE、Number.MIN\_VALUE定义了Number的值边界，所有ECMAScript数字值必须在这两个数中间，计算结果可以除外。
            
        *   无穷大：Infinity Number.POSITIVE\_INFINITY/Number.NEGATIVE\_INFINITY
            
        *   NaN,代表非数
            
    *   "string" 变量是String型
        
        *   双引号与单引号都可，因为没有字符型
            
    *   "object" 变量是一种引用类型或Null类型 null是Null类型的专用值，ECMA将null与undefined（由null派生而来）定义为相等
        
*   引用值：存储在堆中的对象，也就是说，存储在变量处的值是一个指针，指向存储对象的内存地址
    
*   数据类型的相互转换
    
    *   toString()方法
        
        *   Number的toStrng()方法有两种，默认模式与基模式（2，8，16）
            
        *   String类型也有toString方法（伪对象）
            
    *   parseInt()和parseFloat()
        
        *   从位置0查看每一个字符，直到找到第一个非有效字符为止，将前面的值转为数字
            
        *   parseInt可以转化8或16进制字符，parseFloat不可以
            
    *   强制类型转换
        
        *   Boolean(value)
            
        *   Number(value)
            
        *   String(value)
            
*   引用类型（实际上是对象）：
    
    *   var o = new Object();
        
    *   Object类：
        
        *   属性：
            
            *   Constructor——对创建对象的函数的引用（指针）
                
            *   Prototype——对该对象的对象原型的引用
                
        *   方法：
            
            *   HasOnwnProperty——判断对象是否有某个特定的属性。必须用字符串指定该属性
                
            *   IsPrototypeOf——判断该对象是否为另一个对象的原型
                
            *   PropertyIsEnumerable——判断对象是否可以有for...in语句进行枚举
                
            *   ToString——返回对象的原始字符串表示
                
            *   ValueOf——返回最适合该对象的原始值
                
    *   Boolean类
        
    *   Number类
        
        *   toFixed()方法，返回具有指定位数小数的数字的字符串表示 99 —toFixed(2)—99.00
            
        *   toExponential()方法，返回科学记数法表示的数字的字符串形式99—toExponential(1)—9.9e+1
            
        *   toPrecision()方法，根据最有意义的方式返回数字的预定形式或指数形式
            
        *   以上方法都会进行舍入操作，以使用正确的小数位数正确地表示一个数
            
    *   String类
        
        *   属性length，返回字符串中字符的个数（即使字符串中包含双字节 字符，每个字符也只算是一个字符）
            
        *   charAt()返回包含指定位置字符的字符串
            
        *   charCodeAt()返回字符代码
            
        *   concat()用于将一个或多个字符串连接在String对象的原始值上，返回String原始值，原对象保持不变
            
        *   indexOf()返回指定子串在字符串中的位置或-1，lastIndexOf()从结尾开始检索
            
        *   localeCompare()对字符串值进行排序，该方法需要传参作为比较的字符串：
            
            *   之前，返回负数
                
            *   等于，返回0
                
            *   之后，返回正数
                
        *   从子串创建字符串值：
            
            *   slice()
                
            *   substring()
                
            *   两个方法返回的都是要处理的字符串的子串，都接受一个或两个参数，第一个参数为起始位置，第二个参数为终止位置。
                
            *   对于负数参数，slice()会用字符串的长度加上参数，sustring()则将其当作0处理
                
        *   toLowerCase()、toLocalLowerCase()、toUpperCase()、toLocaleUpperCase()
            
        *   String类的所有属性和方法都可以利用到String原始值上，因为它是伪对象
            
*   instanceof()函数
    
    用于识别正在处理的对象的类型，instanceof要求开发者明确地确认对象为某特定类型
    

#### 运算符

##### 一元运算符

*   delete：删除对以前定义的对象属性或方法的引用
    
*   void：对任何值都返回undefined。用于避免输出不应该输出的值：
    
    ​ `<a href="javascript:void(window.open('about:blank'))"></a>`
    
    ​ 没有返回值的函数返回的都是undefined
    
*   前增量/前减量运算符
    
*   后增量/后减量运算符
    
    *   前...表示先进行增/减运算再进行表达式运算，后则相反
        
    *   `var num = 10; alert(num--);//结果为10 alert(--num);//结果为8`
        
*   一元加法和一元减法
    
    *   一元加法或一元减法会把字符串转换为近似的数字
        
*   位运算符
    
    ​ ECMA使用32位存储有符号整数，第31位作为符号位，使用补码存储
    
    *   NOT运算符(~)：将数字的二进制码取反码，最后转化为浮点数输出
        
    *   AND运算符(&)：25 & 3 = 0....11001 &0....00011 = 00001 = 1
        
    *   OR运算符(|)：25 | 3 = 0....11001 | 0....00011 = 11011 = 27
        
    *   XOR运算符(^)：25^3 = 0....11001^0....00011 = 0....11010 = 26
        
    *   左移运算符(<<)2<<5 = 64 保留符号位
        
    *   右移运算符(>>)64>>5 = 2 , 无符号右移为>>>
        
*   Boolean运算符
    
    *   NOT运算符(!)：
        
        *   对象，返回false
            
        *   0，返回true
            
        *   0以外数字，返回false
            
        *   null，返回true
            
        *   NaN，true
            
        *   undefined，发生错误
            
    *   逻辑AND运算符&&（懒运算）:
        
        *   如果一个运算数是对象，另一个是Boolean值，返回该对象
            
        *   如果两个运算数都是对象，返回第二个对象
            
        *   如果某个运算数是null，返回null
            
        *   如果某个运算数是NaN，返回NaN
            
        *   如果某个运算数是undefined，发生错误
            
    *   逻辑OR运算符||（懒运算）:
        
        *   如果一个运算数是对象，另一个是Boolean值，返回该对象
            
        *   如果两个运算数都是对象，返回第二个对象
            
        *   如果某个运算数是null，返回null
            
        *   如果某个运算数是NaN，返回NaN
            
        *   如果某个运算数是undefined，发生错误
            
*   乘性运算符
    
    *   乘法运算符
        
        *   某个运算数是NaN,结果为NaN
            
        *   Infinity乘以0，结果为NaN
            
        *   Infinity乘以0以外任何数字，结果为Infinity 或 -Infinity，由第二个运算数的符号决定
            
        *   Infinity乘Infinity，结果为Infinity
            
    *   除法运算符
        
        *   Infinity除Infinity ， 结果为NaN
            
        *   0除一个非无穷大的数字，结果为NaN
            
        *   某个运算数字为NaN，结果为NaN
            
    *   取模运算符
        
        *   若被除数为0或者除数为0，结果为NaN
            
    *   加性运算符
        
        *   若两个运算数都是字符串，把第二个字符串连接到第一个字符串
            
        *   若只有一个是字符串，把另一个运算数转换为字符串，结果为两个字符串连接的字符串
            
    *   减法运算符
        
        *   某个运算符不是数字，结果为NaN
            
*   关系运算符
    
    *   '>'
        
    *   <
        
    *   <=
        
    *   '>='
        
    *   对两个字符串应用关系运算符，则会用ascii码进行对比
        
*   等性运算符
    
    *   等号和非等号
        
        *   ==
            
        *   !=
            
        *   null与undefined相等，若某个运算数为NaN，返回false
            
        *   若两个运算数都为对象，那么比较的是他们的引用值
            
    *   全等号和非全等号
        
        *   ===
            
        *   !==
            
        *   与等号和非等号相比，在检查相等性的时候，不会进行类型转换
            
    *   条件运算符：三元运算符 `var iMax = (iNum1>iNum2)?iNum1:iNum2`
        
*   赋值运算符
    
    *   =
        
        *   复合赋值运算符（常用）：*= 、/=、%=、+=、-=、<<=、>>=、>>>=
            
*   逗号运算符
    
    *   `var iNum = 1,iNum2 = 2,iNum3 = 3`
        

#### 语句

*   条件语句
    
*   迭代语句
    
    *   do-while语句
        
    *   while语句
        
    *   for语句
        
    *   for-in语句
        
        `for(property in expression) statement`
        
*   有标签的语句(给语句加标签，方便以后调用)
    
    label：statement `start:var count=10`
    
*   break语句和continue语句
    
    *   break和continue可以与标签语句配合使用，跳转到对应语句
        
*   with语句
    
    *   用于设置代码在特定对象中的作用域 with(expression) statement;
        
    *   with 语句可以方便地用来引用某个特定对象中已有的属性，但是不能用来给对象添加属性。要给对象创建新的属性，必须明确地引用该对象。  
        with(object instance)  
        {  
         //代码块
        }  
         有时候，我在一个程序代码中，多次需要使用某对象的属性或方法，照以前的写法，都是通过:对象.属性或者对象.方法这样的方式来分别获得该对象的属性和方法，着实有点麻烦，学习了with语句后，可以通过类似如下的方式来实现：
        with(objInstance)  
        {  
         var str = 属性1;
        .....  
        } 去除了多次写对象名的麻烦。  
        
*   switch语句