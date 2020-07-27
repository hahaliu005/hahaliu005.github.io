---
title: The Use Of Stack
tags:
  - C
  - DataStructure
date: 2018-08-06
---

About the use of stack.

<!-- more -->

```c
#include "header.c"
typedef char SElemType;
#include "LinkStack.c"
 
//conversion decimal to octal
void conversion(){
	LinkStack S;
	int N,e;
	InitStack(&S);
	printf("Enter a decimal : ");
	scanf("%d",&N);
	printf("The decimal you entered is :%d\n",N);
	while(N){
		printf("Push %d in stack s\n",N%8);
		Push(S,N%8);
		N = N/8;
		printf("Now N is %d\n",N);
	}
	printf("The decimal you entered conversion to octal is : ");
	while(! StackEmpty(S)){
		Pop(S,&e);
		printf("%d " , e);
	}
}
//line edit
void LineEdit(){
	LinkStack S,TS;
	char ch , c;
	InitStack(&S);
	InitStack(&TS);
	printf("Enter some character (#:back @:clear ):\n");
	ch = getchar();
	while(ch != EOF){
		while(ch != EOF && ch != '\n'){
			switch(ch){
				case '#':{
					Pop(S,&c);
					break;
				}
				case '@':{
					ClearStack(S);
					break;
				}
				default:{
					Push(S,ch);
					break;
				}
			}
			ch = getchar();
		}
		printf("Now the line stack is:");
		while(! StackEmpty(S)){
			Pop(S,&c);
			Push(TS , c);
			printf("%c ",c);
		}
		printf("\n");
		ClearStack(S);
		if(ch != EOF){
			ch = getchar();
		}
	}
	printf("The total Stack is:\n");
	while(! StackEmpty(TS)){
		Pop(TS,&c);
		printf("%c ",c);
	}
	DestroyStack(&S);
}
 
 
 
void main(){
	//when test one function , commond the rest function 
	//and change the define type define of SElemType
	LineEdit();
	conversion();
}
```