---
title: Fibonacci
tags:
  - C
  - Algorithm
date: 2018-08-06
---

A gist about Fibonacci 

<!-- more -->

```c
#include<iostream>
using namespace std;
int feb(int n){
	cout<<n<<endl;
	if(n == 0 ){
		return 0;
	}
	if(n == 1){
		return 1;
	}
	return feb(n - 1) + feb(n - 2);
}
int f(int n){
	int tp;
	if(n == 1 || n == 2){
		return 1;
	}
	if(n == 0){
		return 0;
	}
	int temp[2];
	temp[0] = 1;
	temp[1] = 1;
	for(int i = 2 ; i < n ; i++){
		tp = temp[0] + temp[1];
		temp[0] = temp[1];
		temp[1] = tp;
	}
	return tp;
}
int main(){
	cout<<f(5)<<endl;
	cout<<"please enter a int number:"<<endl;
	int f;
	cin>>f;
	do 
	{
		cout<<"the feb number is:"<<endl;
		cout<<feb(f)<<endl;
		cout<<"please enter a int number:"<<endl;
		cin>>f;
	} while (f > 0);
	
 
	cin.get();
	cin.get();
	return 0;
}
```