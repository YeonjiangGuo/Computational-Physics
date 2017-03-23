# homework 1
## problem2
* code for problem 2
```
#include <stdio.h>
#include <stdlib.h>
#include <iostream>
#include <cmath>
#include <fstream>
#define N 100              // 定义循环次数 
#define T0 20              // 定义循环周期 
#define pi 3.1415926

using namespace std;

double funf(int x,double y1,double y2)
{
	double f;
	f=y2;
	return(f);
}

double fung(int x,double y1,double y2)
{
	double g,q,b,b1;
	int dt;                  //这里定义dt为t对周期T0的余数
	dt=x%T0; 
	q=0.5;
	b=1.35;
	g=b*(2*dt/T0-1)-q*y2-sin(y1);
	return(g);
}

int main()
{
	double k[2][4],y[2][N],alpha[4]={1.0/6,1.0/3,1.0/3,1.0/6},h;
	int t,i,j;
	ofstream outfile;
	
	h=2*pi/20;  // 定义步长
	y[0][0]=0.0;
	y[1][0]=0.9;
	
	for(t=1;t<N;t++)
	{
		k[0][0]=funf(t,y[0][t-1],y[1][t-1]);
		k[1][0]=fung(t,y[0][t-1],y[1][t-1]);
		
		k[0][1]=funf(t,y[0][t-1]+k[0][0]*h/2,y[1][t-1]+k[1][0]*h/2);
		k[1][1]=fung(t,y[0][t-1]+k[0][0]*h/2,y[1][t-1]+k[1][0]*h/2);
		
		k[0][2]=funf(t,y[0][t-1]+k[0][1]*h/2,y[1][t-1]+k[1][1]*h/2);
		k[1][2]=fung(t,y[0][t-1]+k[0][1]*h/2,y[1][t-1]+k[1][1]*h/2);
		
		k[0][3]=funf(t,y[0][t-1]+k[0][2]*h,y[1][t-1]+k[1][2]*h);
		k[1][3]=fung(t,y[0][t-1]+k[0][2]*h,y[1][t-1]+k[1][2]*h);
		
		y[0][t]=y[0][t-1];
		y[1][t]=y[1][t-1];
		
		for(i=0;i<4;i++)
		{
			y[0][t]=y[0][t]+h*alpha[i]*k[0][i];
			y[1][t]=y[1][t]+h*alpha[i]*k[1][i];
		}
		
		if (y[0][t]>pi)
		y[0][t]=y[0][t]-2*pi;
		else if (y[0][t]<-pi)
		y[0][t]=y[0][t]+2*pi;
		
		cout<<"theta ("<<t<<") = "<<y[0][t]/pi*180<<" "<<"omega ("<<t<<") = "<<y[1][t]<<'\n';  // theta输出结果用角度表示 
	}
	
	outfile.open("homework_02_theta.txt");
	for(j=0;j<N;j++)
		outfile<<y[0][j]<<'\n';
	outfile.close();
	outfile.open("hemework_02_omega.txt");
	for(j=0;j<N;j++)
		outfile<<y[1][j]<<'\n';
	outfile.close();
	
}
```
* figures for different force
![figure 1. b=0.9](http://on7yq9in2.bkt.clouddn.com/homework02_1.jpg)  

![figure 2. b=1.35](http://on7yq9in2.bkt.clouddn.com/homework02_2.jpg)  

If change b from 0.9 to 1.35 will get chaos.
