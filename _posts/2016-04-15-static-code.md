---
layout: post
title: 静态代码质量的几个建议
tags:
    - 代码审核
---



1.	捕捉异常用Logger，不要直接e.printStackTrace();请用Logger，不要直接System.out.println();
避免在正式程序代码中使用 System.out 或 System.err 或者 main方法，如果想测试，则写测试用例junit
Logger日志等级如下：
logger.info 用于线上，记录重要的信息。
logger.debug 用于开发，trycatch异常，比较常用。
logger.warn 用于是否稳定。
logger.error 用于系统错误，属于高级别。
![](http://cdn.jasonsoso.com/2016/sc1.png) 

2.	请用面向接口，比如以下java常用类：Map<String, String> priceUnitsMap = new HashMap<String, String>();Set employees = new HashSet()；![](http://cdn.jasonsoso.com/2016/sc2.png) 

3. 	针对常量，进行比较，请常量放左边，变量放右边进行比较，预防空指针;int类型请用==进行比较对比，String请用equals进行比较对比！![](http://cdn.jasonsoso.com/2016/sc3.png) 

4.	针对方法，请删除无用的参数
![](http://cdn.jasonsoso.com/2016/sc4.png) 

5.	类的命名：必须由大写字母开头而其他字母都小写的单词组成；如果两个单词组合而成，则两个单词首字母为大写，其他为小写，不使用拼音，如DataUtil。
方法的命名：方法名应该是动词，大小写可混用，但首字母应小写。在每个方法名内，大写字母将词分隔，如：send，sendMessage。
变量的命名：变量的名字可大小写混用，但首字符应小写。词由大写字母分隔。
数组的命名：数组应该总是用下面的方式来命名：byte[] buffer;而不是byte buffer[]。
常量的命名：统一大写，复合词则用下划线连接，如：IREE_PORT;
![](http://cdn.jasonsoso.com/2016/sc5.png) 
![](http://cdn.jasonsoso.com/2016/sc5-2.png) 


6.	 删除无用的变量，或者是import，或者是局部变量
![](http://cdn.jasonsoso.com/2016/sc6.png)![](http://cdn.jasonsoso.com/2016/sc6-2.png)![](http://cdn.jasonsoso.com/2016/sc6-3.png) 
	

7.	 一律用大括号括住 if 和 loop 语句
if else 建议写上{}，个人习惯比较清晰和安全。
![](http://cdn.jasonsoso.com/%40%2F2016%2Fsc7.png) 

8. 注释几点说明：
	1.	 内容要简单、明了、含义准确，防止注释的多义性。
	2. 	注释的就近原则。对代码的注释应放在其上方相邻或右方的位置，不可放在下方。
	3. 	面向接口，接口注释采用 /** …… */，在接口注释清楚的前提下对应的实现类可以不加注释。
	4. 	如果方法名称，简单易懂，可以不加注释，方法名称最好能让人通俗易懂。尽量用方法名就能说明你的意义，因为维护注释也需要很大的成本.。
	5. 	常量：常量通常具有一定的实际意义，要定义相应说明。
	6. 	一个方法多次改动，也请维护你的注释。




9. 	SVN几点操作说明
	1. 	在修改代码先，先update。
	2. 	修改后，先测试无BUG，再update，没冲突，再COMMINT。
	3. 	在COMMINT前， 想看是否是自己想提交的代码，可资源对比，是与服务器上代码比较。
	4.	 在每提交代码时，加上注解再提交。


