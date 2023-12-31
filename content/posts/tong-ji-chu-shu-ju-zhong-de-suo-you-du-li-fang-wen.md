---
title: 统计出数据中的所有独立访问
date: 2016-06-10
tags:
  - Algorithm
---


今天做了一道笔试题，看起来特别简单的一道题，却让我做了很久，上网查基础也说明自己基础还是有待加强，主要记录自己的解题思路，题目中出题人提供的数据比较随意，所以得出的结果也比较随意 ：）

## 问题描述

用java代码完成下面题目,在一个日志文件2013-05-30.log中有如下数据


27.19.74.143 - - [30/May/2013:17:38:25 

27.19.74.143 - - [30/May/2013:17:38:22 

27.19.74.143 - - [30/May/2013:17:38:23 

27.19.74.143 - - [30/May/2013:17:38:21 

27.19.74.143 - - [30/May/2013:17:38:28 

27.19.74.143 - - [30/May/2013:16:38:20 

27.19.74.143 - - [30/May/2013:18:38:20 

27.19.74.143 - - [30/May/2013:16:39:20 

27.19.74.143 - - [30/May/2013:16:41:20 

27.19.74.143 - - [30/May/2013:17:42:20 

27.19.74.143 - - [30/May/2013:19:38:21 

27.19.74.143 - - [31/May/2013:12:38:21 

27.19.74.143 - - [31/May/2013:16:38:21 

27.19.74.143 - - [31/May/2013:16:38:22 

27.19.74.143 - - [30/May/2013:14:38:21 

27.19.74.143 - - [30/May/2013:14:38:21 


该文件中的数据是一段截取自web服务器日志中的数据，每一行包含两个信息： 


1.  网站访问者的ip 

2.  网站访问者一次请求的时间 


定义：连续的请求属于一次“独立访问”，如“30/May/2013:17:38:22”和“30/May/2013:17:38:25”两次请求属于同一次独立访问，时间相邻的两次请求如果间隔超过30分钟，则视为分属两次不同的独立访问。


需求：统计出数据中的所有"独立访问"，输出每一次访问的起始请求时间，结束请求时间，及停留时长（毫秒）。


输出结果示例： 


31/May/2013:16:38:21    30/May/2013:16:41:20      181000

## 答案

```Java
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.Locale;
/**
 * @Date 	2016-4-06
 * @author 	Peiel
 * @E-Mail  peielcn@gmail.com
 *
 */
public class Test {
	public static SimpleDateFormat sdf = new SimpleDateFormat("dd/MMMM/yyyy:HH:mm:ss",Locale.US);	//其中月份为英文显示
	public static final long mm_30 = 30*60*1000; 	//30分钟的毫秒数

	/**
* 获取日志文件中的数据，提取出时间，并存放在ArrayList中
* @return	ArrayList
*/
	public static ArrayList<Date> getData(){
ArrayList<Date> data = new ArrayList<Date>();
File file = new File("C:\\2013-05-30.log");
BufferedReader reader = null;
try {
	reader = new BufferedReader(new FileReader(file));
	String tempString = null;
	while ((tempString = reader.readLine()) != null) {
String source = tempString.substring(tempString.indexOf("[")+1, tempString.length()-1);
data.add(sdf.parse(source));
	}
} catch (Exception e) {
	e.printStackTrace();
} finally {
	if (reader != null) {
try {
	reader.close();
} catch (IOException e) {
	e.printStackTrace();
}
	}
}
return data;
	}

	public static void main(String[] args) {
ArrayList<Date> data = getData();
int j = 0;
for (int i = 0; i < data.size()-1; i++) {
	//比较首个时间和第二个时间的时间差，如果小于30分钟，跳出当前循环，然后下次循环比较首个时间和第三个时间的时间差，以此类推，直到大于30分钟跳到else输出，并重置首个时间索引！
	if (Math.abs(data.get(j).getTime()-data.get(i+1).getTime()) < mm_30) {	
continue;
	}else{
System.out.println(sdf.format(data.get(j)) + "	" + sdf.format(data.get(i)) + "	" + Math.abs(data.get(j).getTime()-data.get(i).getTime()));
j = i + 1;
	}
}
	}

}
```

#### 程序运行结果

30/May/2013:17:38:25	30/May/2013:17:38:28	3000

30/May/2013:16:38:20	30/May/2013:16:38:20	0

30/May/2013:18:38:20	30/May/2013:18:38:20	0

30/May/2013:16:39:20	30/May/2013:16:41:20	120000

30/May/2013:17:42:20	30/May/2013:17:42:20	0

30/May/2013:19:38:21	30/May/2013:19:38:21	0

31/May/2013:12:38:21	31/May/2013:12:38:21	0

31/May/2013:16:38:21	31/May/2013:16:38:22	1000
