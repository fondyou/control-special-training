# 第二节：C语言基本语句及运算符

**本节知识点**
 
- for语句
- if else语句
- while语句
- switch语句
- 运算符++

> 以及防止逻辑混乱的技巧（大条件加小条件法则）

!> 注意：当循环出现错误时，首先看它是如何循环的（也就是循环是如何执行的），然后再看内层逻辑

## 一、break、continue在循环中的使用

?> 大家都知道break退出本层循环,continue结束本次循环,注意区别

eg1：
	
	#include <stdio.h>
	
	int main()
	{
		int i,j;
		/**********测试两个for******************/
		for(i=0;i<10;i++)
		{
			for(i=0;j<10;j++)
			{
				if(j == 5)
					break;
				printf("j = %d ",j);		
			}
			if(i == 5)
				break;
			printf("\n");
		} 		//这两个for循环是如何执行的
		
		/********测试while + for***********/	
		j =10;
		while(j--)
		{
			for(i=0;i<10;i++)
			{
				if(i==5)
					break;
			}
			if(j==5)
				break;
		}                             //自己加打印语句看是如何循环的
		return 0;
	}

?> 补充：使用for循环的场合，就是在循环次数已知的情况


## 二、循环中的大条件小条件法则

- 程序要实现一个功能：编写一个函数“456dfa241gafdasd13234fadfa” 找到字符a的个数，如果遇到字母s（统计一串字符字母s前a的个数）

- 明确这两点：程序就好写了
	- 1大的条件就是它要是一个字母，小的条件它是字母a	
	- 2循环退出的条件就是遇到字母s，字符串遍历结束
	
- 在每个关键循环的地方都要加注释，相当于是梳理逻辑

eg2：

	#include <stdio.h>
	
	int find_a(char * str)
	{
		int count = 0；//必须要初始化才行
		while(*str != '\0')
		{
			if(isalpha(*str))         //大条件，判断是否是字母
			{
				if(*str == a)
					count++;
				if(*str == s)//遇到s提前退出循环        
					breakl; 
			}
			str++;
		}
		return count；
	}
	int main()
	{
		char str[]={"456dfa241gafdasd13234fadfa"};
		printf("number of a =%d \n",find_a(str));
		return 0;
	}


## 三、switch与if else 实现分支语句+if中会忽略的一个问题

**switch使用模板：**

	switch ( XXX )            //针对情况固定且有限的情况
	{
		case a:
		....
		break;
		case b:
		....
		break;
		case c:
		....
		break;
		defualt :     //这一项必须写
	}

**if else使用模板：**

	if(count ++)  //有时会忽略++操作了count变量，只注意看count ++是否为真这个条件了
	else if()
	else
