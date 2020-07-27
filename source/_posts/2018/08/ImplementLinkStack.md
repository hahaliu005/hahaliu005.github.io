---
title: Implement Link Stack
tags:
  - C
  - DataStructure
date: 2018-08-06
---

About the implementation of link stack.

<!-- more -->

LinkStack.c
```c
typedef SElemType ElemType;
 
#include "SLinkList.c"
typedef LinkList LinkStack;
#define InitStack InitList
#define DestroyStack DestroyList
#define ClearStack ClearList
#define StackEmpty ListEmpty
#define StackLength ListLength
#define GetTop GetFirstElem
#define Push HeadInsert
#define Pop DeleteFirst
Status StackTraverse(LinkStack S , void(*visit)(SElemType)){
	SElemType e;
	LinkStack temp,p = S;
	InitStack(&temp);
	while(p->next){
		GetTop(p , &e);
		Push(temp , e);
		p = p->next;
	}
	ListTraverse(temp,visit);
	return OK;
}
```

LinkStack_main.c
```c
#include "header.c"
typedef int SElemType;
#include "LinkStack.c"
 
void print(SElemType c){
	printf("%d ",c);
}
 
void main(){
	int j;
	LinkStack s;
	SElemType e;
	if(InitStack(&s)){
		for(j = 1;j<=5;j++){
			Push(s,2*j);
		}
	}
	printf("The elements from base to top is:");
	StackTraverse(s,print);
	Pop(s,&e);
	printf("The element be poped from top is:%d\n",e);
	printf("Is stack empty:%d(1:yes 0:no)\n",StackEmpty(s));
	GetTop(s,&e);
	printf("Now the top element is %d , the length is %d\n",e,StackLength(s));
	ClearStack(s);
	printf("After clear stack,is stack empty:%d(1:yes 0:no) , the length is %d\n",StackEmpty(s),StackLength(s));
	printf("Is destroyed:%d(1:yes 0:no)\n",DestroyStack(&s));
}
```