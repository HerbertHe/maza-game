# 走迷宫游戏

~~这个游戏是老师布置的一个项目，基本所有的代码都是一个人设计框架和写的，最主要的部分也就是逻辑调用的部分，代码退出程序的时候仍然是有问题，上传之后考虑重写那一部分，最核心的部分莫过于光标定位和数组值的检测，在跟函数调用的部分确实遇到了逻辑错误。
另外有的要求是，实现游戏的保存与继续，这个跟文件的读写有关系，在前期的设计之中跟对抗这一发生了冲突，后来想到了考虑在之后维护（这个项目通过了答辩就不想做了）。~~

~~## 游戏保存和继续的设计思路：~~

~~在开始游戏之后加上按键检测继续，然后打印迷宫数组和已经继续的位置坐标，到调用手动走的部分进行检测，按键检测只要检测到‘S’or‘s’立马save，然后退出检测循环（检测模块要适当修改）~~

~~## 然后就是模块化集成的问题，里面重复的检测部分可以重新写函数传递参数调用，就算加了保存、继续和回放应该也不超过1000行~~


## 文档的补充说明

首先感谢bilibili小伙伴的支持，我不是经常更新视频，码农不太会说话，有bug直接发邮件给我或者留issues就好了，，能有点赞、收藏、投币、转发就更好了！！！

## 怎么才能使用

> 因为时间太长了，我已经删掉了原工程文件！没有备份地图文件！！！

第40行到第122行是地图读取函数！因为结构相同，我就用mapread1（简单地图）函数举例，下面的mapread2（一般地图）和mapread3（困难地图）

```c
void mapread1(int sizex,int sizey)                                    //Easy地图
{
	int i,j,ch=0;
	FILE *fp;
	int hang,lie;
	lie=40;
	hang=10;
	SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_INTENSITY |FOREGROUND_RED | FOREGROUND_GREEN | FOREGROUND_BLUE);
	gotoxy(lie,hang);printf("Map Choose");hang++;hang++;
	gotoxy(lie,hang);printf("[1]地图Map1");hang++;hang++;
	gotoxy(lie,hang);printf("[2]地图Map2");hang++;hang++;
	gotoxy(lie,hang);printf("[3]地图Map3");
	ch=getch();
	system("cls");
	if(ch==49) fp=fopen("C:\\Users\\Herbert\\Desktop\\map\\Easy\\11.txt","r");
	if(ch==50) fp=fopen("C:\\Users\\Herbert\\Desktop\\map\\Easy\\12.txt","r");
	if(ch==51) fp=fopen("C:\\Users\\Herbert\\Desktop\\map\\Easy\\13.txt","r");
	for(i=0;i<sizex+1;i++)
	{
		for(j=0;j<sizey+1;j++)
		{
			n[i][j]=fgetc(fp);
		}
	}
	fclose(fp);
	printnxy(sizex+1,sizey+1);
}
```

> 不管是自动走还是手动，加载的地图同一类型都是一样的！这个mapread函数其实就是加载地图的函数

```c
  if(ch==49) fp=fopen("C:\\Users\\Herbert\\Desktop\\map\\Easy\\11.txt","r");
	if(ch==50) fp=fopen("C:\\Users\\Herbert\\Desktop\\map\\Easy\\12.txt","r");
	if(ch==51) fp=fopen("C:\\Users\\Herbert\\Desktop\\map\\Easy\\13.txt","r");
```

> ch的值对应的其实就是ASCII码，选择对应的地图，这里三个mapread函数都是提供了三个地图！！这个是可以添加和更改的！！！

## 地图的大小

仍然以简单地图为例！！代码的第210行开始

```c
int Easy()
{
	void mapread1(int,int);
	int a,b,Step=0;
	int ch=0;
	mapread1(8,8);
	printf("Step:%d",Step);
	a=1;b=0;
	gotoxy(a,b);
	while(1)                                                                                     //逻辑分析一下，定位不能动
	{
		while (1)
		{
			if(ch=getch())     //方向键读取
			{
				if(ch==72&&(n[b-1][a]==48||n[b-1][a]==57))  //up
				{
					n[b][a]=48;
					gotoxy(a,b);
					printf(" ");
					b--;
					n[b][a]=54;                       //*定义为6
					gotoxy(a,b);
					printf("*");
					Step++;
					break;
				}
				if(ch==80&&(n[b+1][a]==48||n[b+1][a]==57))  //down
				{	
					n[b][a]=48;
					gotoxy(a,b);
					printf(" ");
					b++;
					n[b][a]=54;
					gotoxy(a,b);
					printf("*");
					Step++;
					break;
				}
				if(ch==75&&(n[b][a-1]==48||n[b][a-1]==57))   //left
				{
					n[b][a]=48;
					gotoxy(a,b);
					printf(" ");
					a--;
					n[b][a]=54;
					gotoxy(a,b);
					printf("*");
					Step++;
					break;
				}
				if(ch==77&&(n[b][a+1]==48||n[b][a+1]==57))  //right
				{
					n[b][a]=48;
					gotoxy(a,b);
					printf(" ");
					a++;
					n[b][a]=54;
					gotoxy(a,b);
					printf("*");
					Step++;
					break;
				}
			}
		}

		gotoxy(0,9);

		printf("Step:%d",Step);

		gotoxy(a,b);

		if(n[7][6]==54)
		{
			gotoxy(0,11);
			printf("successful");
			break;
		}
	}
	return Step;
}
```

> 其中的mapread1就是调用上面的地图读取函数的！！你也可以在这知道地图的大小！

补：其实起点，出口是定好的！不确定的出口我记得我当时也想了些办法，后来太折腾了时间不够就没做！出口就是那个successful位置的if条件！！！所以出口位置就是(8, 7)    **为什么(7,6)对应(8,7)是因为数组的开始其实是0！！！0~7八行！！

a,b分别对应的是列和行！！所以这个起点(1, 0)就是第二列，第一行！！

## gotoxy是dos界面的打印定位函数！

## 如何制作地图

> 第22行的printxy函数！

```c
void printnxy(int printx,int printy)                                 //数组打印函数
{
	int i,j;
	for(i=0;i<printx;i++)
	{
		for(j=0;j<printy;j++)
		{
			if(n[i][j]==49)
				printf("M");
			if(n[i][j]==48||n[i][j]==57)
				printf(" ");
			if(n[i][j]==54)
				printf("*");
		}
		printf("\n");
	}
}
```

其中的if条件就是关键点！！！！

* 49对应的是数字1
* 48对应的是0 
* 57对应的是9 
* 54对应的是6

这个的意思就是1就是迷宫的墙，0和9代表的其实是路，但是9是终点！6代表的是你所在的位置\*！！

> 这里补充一下！那个9本来就可以用来换终点的位置！这个有兴趣可以自己改！我记得我当时是搞过，，忘记了

## 那么我们现在来说说怎么做地图才能友好玩耍

首先，请仔细看我上面写的内容！！！

以简单地图为例，，起点 **第二列第一行** ，这个画地图就是6，终点 **第八行第七列** ，也就是9！其他的就是0，1构成路径，我举个例子

* 16111111
* 10000111
* 11110001
* 11111191

然后再更改mapread函数的路径就好了！！

## 总结

在简单，一般和困难的函数中找到起点和终点还有地图的大小！！然后6起点，9终点，1是墙，0是路！！这样就可以了！！然后更改readmap的地图路径地址！！

## 说说自动走！！

尽量不要多路径！自动走用的是深度优先算法，单用这个算法会有一些问题！不推多路径！！！
