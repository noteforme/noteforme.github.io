---
title: Regular
comments: true
date: 2020-07-20 17:07:08
tags:
categories: JAVA
---



#### Tool

https://regex101.com/

https://ihateregex.io/expr/password



#### ASCII

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwMTE5MTAyNjAxNzA5?x-oss-process=image/format,png)

读ASCII表 先读 “竖” 、后读 “横”
例如：A , [二进制](https://so.csdn.net/so/search?q=二进制&spm=1001.2101.3001.7020)是 0100 0001 十进制是 2^6 + 1 = 65 十六进制 41

F : 二进制是 0100 0110 十进制是 2^6 + 2^2+2=70 十六进制 46



##### String中的数字转int

```kotlin
val digits = "23"
val c = digits[0] - '0' // 50 - 48
println(digits[0].code) // ASCII是 50
println('0'.code)       //ASCII是 48
```




#### 正则意义对照表格

https://www.jb51.net/shouce/jquery1.82/regexp.html

https://baixin.ink/2016/03/21/regular-expression/

https://www.bilibili.com/video/BV1Eq4y1E79W?p=4

https://deerchao.cn/tutorials/regex/regex.htm

https://deerchao.cn/tools/wegester/

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions

https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285

### 语法

#### 字符

##### 普通字符

字母、数字、汉字、下划线、以及没有特殊定义的标点符号都是普通字符。直接匹配与之相同的字符

-简单的转义字符

| \n   | 换行符 |
| ---- | ------ |
| \t   | 制表符 |
| \\   | \本身  |



其他都是类似  \^ \$  \( 这样匹配这些字符本身的（还有    ) { } ? + * | [ ]    ）

##### 标准字符集合

 能够与多种字符匹配的表达式  注意区分大小写 大写是相反的意思

| \d   | 任意一个数字，0-9中任意一个     \D 表示所有非数字            |
| ---- | ------------------------------------------------------------ |
| \w   | 任意一个字母或下划线，也就是A-Z、a-z、0-9、_ 、中的任意一个  |
| \s   | 包括空格、制表符、换行符等空白字符中的任意一个               |
| .    | 小数点可以匹配任意一个字符（除了换行符）。如果要匹配包括“\n”在内的所有字符，一般用 [\s\S] |
| \S   | 匹配任何非**空白**字符。等价于[^ \f\n\r\t\v]。 (注意是空白字符，不是空格) |



##### 自定义字符集合

[]方括号匹配方式 ，能够匹配方括号中任意一个字符。**^符号在方括号里表示 取反  方括号外表示零宽标记**，见后面

| [ab5@]    | 匹配 “a”或“b”或“5”或“@”                |
| --------- | -------------------------------------- |
| [^abc]    | 匹配“a”、“b”、”c“之外的任意一个字符    |
| [f-k]     | 匹配“f”-“k”之间的任意一个字母          |
| [^A-F0-3] | 匹配“A”-“F”，“0”-“3”之外的任意一个字符 |

* 正则表达式的特殊符号，被包含到中括号中则失去其特殊意义（就表示自身这个字符），除了^,-之外。

* 标准字符集合，除小数点外，如果被包含于中括号，自定义字符集合将包含该集合。

  比如： [\d.\-+]将匹配：数字、小数点、+、- 本身意义，除非加转义字符\ 

  

  上面两句一个意思呀

#### 量词Quantifier

修饰匹配次数的特殊符号

| {n}   | 表达式重复n次                     | 如：\d{6}  匹配6位数字  \d\d{6} 匹配7位数字  {\d\d}{6} 匹配12位数字 |
| ----- | --------------------------------- | ------------------------------------------------------------ |
| {m,n} | 表达式至少重复m次，最多重复n次    |                                                              |
| {m,}  | 至少重复m次                       |                                                              |
| ？    | 匹配表达式0次或1次 ，相当于{0，1} | 如：a\d{0,1}b  a\d?b  是等价的                               |
| +     | 表达式至少出现1次,相当于{1，}     | 如：a\d+b                                                    |
| *     | 表达式不出现或出现任意次          | 相当于{0，}                                                  |

匹配次数中的贪婪模式 （匹配字符数越多越好，默认的）如\d{3,6} 会优先匹配 6位数字

匹配次数中的非贪婪模式 （匹配字符越少越好，修饰匹配次数的特殊符号后再加上一个“？”号）

 

#### 字符边界(零宽，不占长度)

-（本组标记匹配的不是字符而是位置，符合某种条件的位置）

| ^    | 匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在方括号表达式中使用时，表示不接受该方括号表达式中的字符集合。要匹配 ^ 字符本身，请使用 \^。 |
| ---- | ------------------------------------------------------------ |
| $    | 与字符结束的地方匹配                                         |
| \b   | 匹配一个单词边界                                             |

^ []部分也有描述.

https://www.runoob.com/regexp/regexp-syntax.html



REG : John\b

John 匹配 可以看到 John_	下划线_就是\b的位置，他的左侧n 匹配，但是右侧空格不匹配 \w,所以就是不全是\w,所以匹配。



-\b匹配这样一个位置：前面的字符和后面的字符不全是\w （全是的就不匹配）

如：^i  匹配 i开头的位置    i$ 匹配i结束的位置  hello\b  匹配前后不全是\w表示的hello 

 

#### 匹配模式

IGNORECASE

忽略大小写 默认情况下正则表达式区分大小写(Case Insensitive)

SINGLELINE

单行模式  整个文本看作一个字符串，只有一个开头，一个结尾。使得小数点“.”可以匹配包含换行符（\n）在内的任意字符

MULTILINE

多行模式 每行都是一个字符串，都有开头和结尾。在指定了此模式后，如果需要仅匹配字符串开始和结束位置，可以使用\A和\Z

如：\Ai匹配 第一行第一个i     ^i 匹配每一行第一个的i   i$匹配每一行的最后一个i  i\Z  匹配整个文档最后一个i

 

#### 选择符和分组

| 表达式                  | 作用                                                         |                 |
| ----------------------- | ------------------------------------------------------------ | --------------- |
| \| 分支结构             | 左右两边表达式之间 “或” 关系，匹配左边或者右边               | 表示“或”   如 h |
| ()   捕获组             | (1) 在被修饰匹配次数的时候，括号中的表达式可以作为整体被修饰<br/><br/> (2)取匹配结果的时候，括号中的表达式匹配到的内容可以被单独地编号    <br/><br/> (3)每一对括号会被分配一个编号，使用（）的捕获根据左括号的顺序从1开始自动编号。捕获元素编号为零的第一 个捕获是由整个正则表达式模式匹配的内容 |                 |
| (?:Expression) 非捕获组 | 一些表达式中，不得不使用（），但又不需要保存（）中的子表达式匹配的内容，这时可以用非捕获组来抵消使用（）带来的副作用。节省内存。 |                 |

 


#### 反向引用（\nnn）\nnn表示可以匹配多位数字

-每一对（）会分配一个编号，使用（）的捕获根据左括号的顺序从1开始自动编号。

-通过反向引用，可以对分组已捕获的字符串进行引用。

  如  ([a-z]{2})\1  中的\1 代表第一个组里的内容,匹配到gogo，toto，dodo这样的字符串。

（）（）（） 捕获的内容分别用\1 \2 \3  编号以左括号为准。

 

#### 预搜索（零宽断言）（也叫环视）

1. 只进行子表达式的匹配，匹配内容不计入最终的匹配结果，是零宽度。
2. 这个位置应该符合某个条件。判断当前位置的前后字符，是否符合指定的条件，但不匹配前后的字符。**是对位置的匹配**。
3. 正则表达式匹配过程中，如果子表达式匹配到的是字符内容，而非位置，并被保存到最终的匹配结果中，那么就认为这个子表达式是占有字符的；如果子表达式匹配的仅仅是位置，或者匹配的内容并不保存到最终的匹配结果中，那么就认为这个子表达式是零宽度的。占有字符还是零宽度，是针对匹配的内容是否保存到最终的匹配结果中而言的。

| 表达式   | 作用                                    |
| -------- | --------------------------------------- |
| (?=exp)  | 断言自身出现的位置的后面能匹配表达式exp |
| (?<=exp) | 断言自身出现的位置的前面能匹配表达式exp |
| (?!exp)  | 断言此位置的后面不能匹配表达式exp       |
| (?<!exp) | 断言此位置的前面不能匹配表达式exp       |



(?<=\d)[a-z]+

22john



如：[a-z]+(?=ing) 匹配ing结尾的字符串，但不匹配ing本身

       [a-z]+(?=\d+) 匹配数字结尾的字符串    
### 常用的一些正则表达式

##### 电话号码验证

（包含固话和手机号，不能确认号码一定存在，只能匹配格式）

（1）电话号码由数字和“-”构成

（2）电话号码为7-8位

（3）如果电话号码中包含有区号，那么区号为3位或4位，首位是0

（4）区号用“-”和其他部分隔开

（5）移动电话号码为11位

（6）11位移动电话号码的第一位和第二位为“13”“15”“18”

 固话   0\d{2,3}-\d{7,9}   手机号    1[35789]\d{9}

 

##### 电子邮箱地址验证

1.用户名：字母、数字、中划线、下划线组成

2.@

3.网址：字母、数字组成

4.小数点。

5.组织域名：2-4位字母组成

不区分大小写

[\w\-]+@[a-z0-9A-Z]+(\.[A-Za-z]{2,4}){1,2} 

如：ssahdahd@sina.com.cn

##### 常用正则表达式列表 

| 表达式                                        | 功能                              |
| --------------------------------------------- | --------------------------------- |
| [\u4e00-\u9fa5]                               | 匹配中文字符                      |
| \n\s*\r                                       | 匹配空白行                        |
| `<(\S*?)[^>]*>.*?</\1>|<.*? />`               | 断言此位置的后面不能匹配表达式exp |
| `^\s*\s*$`                                    | 匹配首尾空白字符                  |
| `\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*` | 匹配Email地址                     |
| `[a-zA-z]+://[^\s]*`                          | 匹配网址URL                       |
| `\d{3}-\d{8}\d{4}-\d{7}`                      | 匹配国内电话号码                  |
| `[1-9][0-9]{4,}`                              | 匹配腾讯QQ号                      |
| `[1-9]\d{5}(?!\d)`                            | 匹配中国邮政编码                  |
| `\d{15}\d{18}`                                | 匹配身份证                        |
| `\d+\.\d+\.\d+\.\d+`                          | 匹配ip地址                        |



括号

https://www.bilibili.com/video/BV1ef4y1U7V4/?spm_id_from=333.788.recommend_more_video.-1

   

### 工作学习中各种环境的使用

##### JAVA程序中使用正则表达式

相关类位于：**java.util.regex包下面**：

1. **类 Pattern：**
    正则表达式的编译表示形式。
    Pattern p = Pattern.compile(r,int); //建立正则表达式，并启用相应模式
2. **类 Matcher：**
    通过解释 Pattern 对 character sequence 执行匹配操作的引擎。
    Matcher m = p.matcher(str); //匹配str字符串
3. **java代码应用正则的查找、分组、替换、分割：**

##### 查找

```java
package com.bjsxt.regex.test;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * 测试正则表达式对象的基本用法
 */
public class RegularTest {
	public static void main(String[] args) {
		//在这个字符串：abcd1234，是否符合指定的正则表达式：\w+
		//表达式对象
		Pattern p = Pattern.compile("\\w+");
		//创建Matcher对象
		Matcher m = p.matcher("abcd￥￥1234");
		//boolean result= m.matches();	//尝试将整个字符序列与该模式匹配
		//System.out.println(result);
		
		//boolean result= m.find();	//该方法扫描输入的序列，查找与该模式匹配的下一个子序列
		
		//System.out.println(m.find());
		//System.out.println(m.group());
		//System.out.println(m.find());
		//System.out.println(m.group());
	
		while(m.find()){
			System.out.println(m.group());	//group(),group(0)匹配整个表达式的子字符串
			System.out.println(m.group(0));
		}
		
	}
}

```



##### 分组

```java
package com.bjsxt.regex.test;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * 测试正则表达式对象中分组的处理
 */
public class Test2 {
	public static void main(String[] args) {
		//在这个字符串：abcd1234，是否符合指定的正则表达式：\w+
		//表达式对象
		Pattern p = Pattern.compile("([a-z]+)([0-9]+)");
		//创建Matcher对象
		Matcher m = p.matcher("abc123%%abcd234%abcde4567");
	
		while(m.find()){
			System.out.println(m.group());	//group(),group(0)匹配整个表达式的子字符串
			System.out.println(m.group(1));
			System.out.println(m.group(2));
		}
		
	}
}

```

find用法

https://www.bilibili.com/video/BV1Eq4y1E79W?p=4

##### 替换

```java
package com.bjsxt.regex.test;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * 测试正则表达式对象的替换操作
 */
public class Test3 {
	public static void main(String[] args) {
		//表达式对象
		Pattern p = Pattern.compile("[0-9]");
		//创建Matcher对象
		Matcher m = p.matcher("abc123%%abcd234%abcde4567");
		//替换
		String result= m.replaceAll("#");
		System.out.println(result);
	}
}

```



##### 分割

```java
package com.bjsxt.regex.test;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * 测试正则表达式对象的分割字符串的操作
 */
public class Test4 {
	public static void main(String[] args) {
		String str = "ab23jk123ji8890123asd";
		String[] arrs = str.split("\\d+");
		System.out.println(Arrays.toString(arrs));
	}
}

```



##### **手写网络爬虫（基本原理&乱码处理）**

```java
package com.bjsxt.regex.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.MalformedURLException;
import java.net.URL;
import java.nio.charset.Charset;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * 网络爬虫取链接	
 */
public class WebSpiderTest {
	
	/**
	 * 获得urlStr对应的网页的源码内容
	 * @param urlStr
	 * @return
	 */
	public static String  getURLContent(String urlStr,String charset){
		StringBuilder sb = new StringBuilder();
		try {
			URL url = new URL(urlStr);
			BufferedReader reader = new BufferedReader(new InputStreamReader(url.openStream(),Charset.forName(charset)));
			String temp = "";
			while((temp=reader.readLine())!=null){
				sb.append(temp);
			}
		} catch (MalformedURLException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return sb.toString();
	}
	
	
	public static List<String> getMatherSubstrs(String destStr,String regexStr){
		Pattern p = Pattern.compile(regexStr);	//取到的超链接的地址
		Matcher m = p.matcher(destStr);
		List<String> result = new ArrayList<String>();
		while(m.find()){
			result.add(m.group(1));
		}	
		return result;
	}
	
	
	public static void main(String[] args) {
		String destStr = getURLContent("http://www.163.com","gbk");
		
		//Pattern p = Pattern.compile("<a[\\s\\S]+?</a>");//取到的超链接的整个内容
		List<String> result = getMatherSubstrs(destStr, "href=\"([\\w\\s./:]+?)\"");
		
		for (String temp : result) {
			System.out.println(temp);
		}
		
	}
}

```



https://blog.csdn.net/qq_39636602/article/details/101236461

https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md

https://regexr.com/





#### Ascii表

https://www.ascii-code.com/

https://www.regular-expressions.info/tutorial.html



#### 密码判断断言

必须包含一个字母或其他的字符

https://www.cnblogs.com/haoyul/p/12059737.html

https://www.bilibili.com/video/BV1nY4y1q7Qr?spm_id_from=333.337.search-card.all.click

https://www.bilibili.com/video/BV1oz4y1C7Uf?p=4

(?=.*[A-Za-z]+)[A-Za-z@\\/`'’‘,\\.\\(\\)\\-]{4,100}$



```
^(?=.*?[A-Z])(?=.*?[^A-Za-z0-9]).{6,12}$

(?=.*?[A-Z])
(?=xxx)是零宽断言，表示后面的字符串必须符合xxx这个正则表达式，但是不消耗字符串，实际匹配字符串的正则是.{6,12}即6到12位字符 
(?=.*?[A-Z])表示后面必须符号.*?[A-Z]这个 ，即必须有大写字母
整个正则表达式表示6到12位字符，必须有大写字母和不是字母数字的字符
```



https://zhidao.baidu.com/question/1609169242401476667.html



#### 正则优先级

下图 从上到下，从左到右的优先级

https://www.runoob.com/regexp/regexp-operator.html

https://zhidao.baidu.com/question/1609169242401476667.html



http://www.rexegg.com/regex-lookarounds.html





### kotlin regular



```kotlin
fun validation(pattern: String, str: String?): Boolean {
    return if (str.isNullOrEmpty()) {
        false
    } else Pattern.compile(pattern).matcher(str).matches()
}
```

