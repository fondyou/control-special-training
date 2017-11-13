# 第三节：结构体对象思想

**本节知识点**
 
- 基本语法
- 对象的编程思想

> 基于对象的编程思想在c语言的是非常常用的，会使程序看起非常简洁

## 一、成员引用符（.  ->）

?>通过结构体变量名字去访问时----用.
?>通过结构体指针去访问成员时----用->

eg1：

```C
#include <stdio.h>

struct stu{
	int a;
	int b;
	int c;
};
int main()
{
	struct stu student;
	struct stu *ptr = NULL;

	student.a = 1;
	student.b = 2;
	student.c = 3;              //自动变量在使用前一定要初始化

	ptr = & student;
	printf("a=%d,b=%d,c=%d\n",student.a,student.b,student.c);
	printf("a=%d,b=%d,c=%d\n",ptr->a,ptr->b,ptr->c);             //两次打印的结构肯定是一样的

	return 0;
}
```

## 二、结构体指针及内存对齐


> 结构体是在计算机中如何存储的
> 结构体指针，顺便讲字节对齐

![图1](http://upload-images.jianshu.io/upload_images/6757403-1da58fa03b39171d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

eg2：

```C
#include <stdio.h>

struct stu{
	int a;
	char b;
	int c;
};
int main()
{
	struct stu student;
	struct stu *ptr = NULL;

	student.a = 0x11223344;
	student.b = 0x11;
	student.c = 0x11223344;           

	printf("room of struct stu =%d \n",sizeof(struct stu));//打印该结构体暂多少个字节，会发现是12个，而不是9个（字节对其的问题）

	ptr = & student;                                                             
	printf("addr  ptr=%p \n",ptr);
	printf("addr  ptr+1=%p \n",ptr+1);                             //会发现两次地址差为12，指针一次偏移了12个字节

	return 0;
}
```

> 链表：（近阶段编程暂时不用，放到c高级去讲）

!> 下面的这些才是重点

结构体的定义：

```C
struct 结构体名
{
	//属性成员
	//关系成员
	//行为成员
}；
```

举例1：描述人这个对象（目前只关心属性成员）

	属性成员：年龄，学号，性别，专业      ---- 对应结构体的变量
	关系成员：属于哪个班，哪个系	          ----对应数据结构中的知识（队列，链表，二叉树等）
	行为成员：考试，上课嘛                        ----对应包含在结构体中的函数
	//这样做的好处：将描述同一个对象弄成一个整体

举例2：描述传感器这个对象（编程中常用嘛）

```C
struct sencer{
	int	RawValue;  //从传感器中采集到的原始数据
	int             DealValue；//将原始数据处理后的数据
	char           SercerState；//传感器当前状态，空闲，忙碌，是否损坏等
}
```

使用场合：只要有几个变量描述同一个东西，那么就把它封装成一个结构体。

## 三、以学生为对象，基于10个学生的编程

编程时首先明确学生对象

```C
struct stu{
	姓名；    
	学号；
	成绩；    
}
```

> 程序功能，找到成绩最好的学生，并打印其姓名与学号


eg3：
```C

#include<stdio.h>

#define NUM 10
struct stu {
	char name[30];
	int    number;
	int    grade ;
};
/*结构体初始化*/
void student_init (struct stu student[],int len) 
{
	int i;
	for(i=0;i<len;i++)
	{
		sprintf(student[i].name,"name%d",i);
		student[i].number = i;
		student[i].grade = i * 10;
	}
}
/*找到成绩最好的学生，并返回该对象*/
struct stu*  find_best_grade(struct stu student[],int len)         //由于使用的是结构体数组，所有要传入数组长度
{
	int i;
	struct stu *PtrRet;
	int GradeMax = 0;
	for(i=0;i<len;i++)
	{
		if( student[i].grade > GradeMax  )
		{
			PtrRet =  &student[i];
			GradeMax =  student[i].grade;
		}			
	}
	return PtrRet ;
}
int main()
{
	struct stu  student[NUM ];
	struct stu *PtrStu;
	student_init(student,NUM) ;
	PtrStu =  find_best_grade(student,NUM);
	printf("name = %s,number = %d,grade = %d\n",(*PtrStu).name,(*PtrStu).number,(*PtrStu).grade);   //将成绩最好的学生信息打印出来
	return 0;
}
```

> 如果没有使用结构体，可想而知30个数组处理起来是多么困难。
> 所谓对象的思想，就是编程时是以学生对象为基本单位进行处理的（而不是以单一变量操作）。操作其中的数据时，十分明确，且便于扩充。
> 使用结构体函数传参时，可以一次传入多个参数



## 四、模拟传感器采集数据

> 以传感器为对象，模拟传感器采集数据，传感器编程的一般流程（今后会常用到）

编程时首先明确传感器对象

```C
struct sencer{
	int	RawValue;  //从传感器中采集到的原始数据
	int             DealValue；//将原始数据处理后的数据
	char           SercerState；//传感器当前状态，空闲，忙碌，是否损坏等
}
void sencer_init();
void sencer_get_date();
void sencer_get_staute();
void sencer_date_deal();      //这些就是这个传感器对象的行为成员，可以放到结构体里面去
```

