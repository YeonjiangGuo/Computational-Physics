# homework 1
## problem 1
* result for Euler :

y(0) = 1  y'(0) = 1  
y(0.1) = 1.1  y'(0.1) = 0.918182  
y(0.2) = 1.19182  y'(0.2) = 0.856197  
y(0.3) = 1.27744  y'(0.3) = 0.807748  
y(0.4) = 1.35821  y'(0.4) = 0.769203  


* result for trapezoidal :

y(0) = 1  y'(0) = 0.959091
y(0.1) = 1.09591  y'(0.1) = 0.881875  
y(0.2) = 1.1841  y'(0.2) = 0.821048  
y(0.3) = 1.2662  y'(0.3) = 0.771588  
y(0.4) = 1.34336  y'(0.4) = 0.730418  


* result for predictor(Euler)-corrector(trapezoidal) :

y(0) = 1  y'(0) = 0.956558  
y(0.1) = 1.09566  y'(0.1) = 0.879378  
y(0.2) = 1.18359  y'(0.2) = 0.818469  
y(0.3) = 1.26544  y'(0.3) = 0.768819  
y(0.4) = 1.34232  y'(0.4) = 0.727357  

* code for problem 1
...

#include <stdio.h>  
#include <stdlib.h>  
#include <math.h>  
#include <iostream>  
#define N 5  

using namespace std;  

double funy_Euler(double x,double y1)  
{  
	double y2;  
	y2=y1-2*x/y1;  
	return(y2);  
}  

double funy_trapezoidal(double h,double x,double y1)  
{  
	double y2[3],yh;  
	y2[0]=funy_Euler(x,y1);  
	yh=y1+y2[0]*h;  
	y2[1]=funy_Euler(x+h,yh);  
	y2[2]=(y2[0]+y2[1])/2;  
	return(y2[2]);  
}  

double funy_predictor_corrector(double h,double x,double y1)  
{  
	double y2[3],yh[2];  
	  
	y2[0]=funy_Euler(x,y1);  
	yh[0]=y1+y2[0]*h;  
	y2[1]=funy_Euler(x+h,yh[0]);  
	y2[2]=(y2[0]+y2[1])/2;  
	  
	do  
	{  
		yh[0]=y1+y2[2]*h;  
		y2[1]=funy_Euler(x+h,yh[0]);  
		y2[2]=(y2[0]+y2[1])/2;  
		yh[1]=y1+y2[2]*h;  
	}  
	while((yh[1]-yh[0])>0.00000001||(yh[0]-yh[1])>0.00000001);  
	return(y2[2]);  
}  
  
int main()  
{  
	double y[2][N+1],h;  
	int i;  
	y[0][0]=1.0;  
	h=0.1;  
	for(i=1;i<=N;i++)                                                           // use Eurel method  
	{  
		y[1][i-1]=funy_Euler((i-1)*h,y[0][i-1]);  
		y[0][i]=y[0][i-1]+h*y[1][i-1];  
	}  
	cout<<"result for Euler : "<<'\n'<<'\n';  
	
	for(i=0;i<N;i++)  
	{  
		cout<<"y("<<i*h<<") = "<<y[0][i]<<"  y'("<<i*h<<") = "<<y[1][i]<<'\n';  
	}  
	for(i=1;i<=N;i++)                                                           // use trapezoidal method  
	{  
		y[1][i-1]=funy_trapezoidal(h,(i-1)*h,y[0][i-1]);  
		y[0][i]=y[0][i-1]+h*y[1][i-1];  
	}  
	cout<<'\n'<<'\n'<<"result for trapezoidal : "<<'\n'<<'\n';  
	
	for(i=0;i<N;i++)  
	{  
		cout<<"y("<<i*h<<") = "<<y[0][i]<<"  y'("<<i*h<<") = "<<y[1][i]<<'\n';  
	}  
	for(i=1;i<=N;i++)                                                            // use predictor(Euler)-corrector(trapezoidal)  
	{  
		y[1][i-1]=funy_predictor_corrector(h,(i-1)*h,y[0][i-1]);  
		y[0][i]=y[0][i-1]+h*y[1][i-1];  
	}  
	cout<<'\n'<<'\n'<<"result for predictor(Euler)-corrector(trapezoidal) : "<<'\n'<<'\n';  
	
	for(i=0;i<N;i++)  
	{  
		cout<<"y("<<i*h<<") = "<<y[0][i]<<"  y'("<<i*h<<") = "<<y[1][i]<<'\n';  
	}  
}  
...
