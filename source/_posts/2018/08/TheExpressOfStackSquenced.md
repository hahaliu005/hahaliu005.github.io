---
title: The Express Of Stack Squenced
tags:
  - C
  - DataStructure
date: 2018-08-05
---

Gist for stack squenced

<!-- more -->

SqStack.c
```c
#define STACK_INIT_SIZE 100
#define STACKINCREMENT 10
typedef struct{
	SElemType *base;
	SElemType *top;
	int stacksize;
}SqStack;
 
Status InitStack(SqStack *S){
	(*S).base = (SElemType *)malloc(STACK_INIT_SIZE * sizeof(SElemType));
	if(!(*S).base){
		return OVERFLOW;
	}
	(*S).top = (*S).base;
	(*S).stacksize = STACK_INIT_SIZE;
	return OK;
}
 
Status DestroyStack(SqStack *S){
	free((*S).base);
	(*S).base = NULL;
	(*S).top = NULL;
	(*S).stacksize = 0;
	return OK;
}
 
Status ClearStack(SqStack *S){
	(*S).top = (*S).base;
	return OK;
}
 
Status StackEmpty(SqStack S){
	if(S.top == S.base){
		return TRUE;
	}
	else{
		return FALSE; 
	}
}
 
int StackLength(SqStack S){
	return S.top - S.base;
}
 
Status GetTop(SqStack S , SElemType *e){
	if(S.top == S.base){
		return ERROR;
	}
	*e = *(S.top - 1);
	return OK;
}
 
Status Push(SqStack *S , SElemType e){
	if((*S).top - (*S).base >= (*S).stacksize){
		(*S).base = (SElemType *)realloc((*S).base , ((*S).stacksize + STACKINCREMENT) * sizeof(SElemType));
		if(!(*S).base){
			return OVERFLOW;
		}
		(*S).top = (*S).base + (*S).stacksize;
	}
	*(*S).top++ = e;
	(*S).stacksize += STACKINCREMENT;
	return OK;
}
 
Status Pop(SqStack *S , SElemType *e){
	if((*S).top == (*S).base){
		return ERROR;
	}
	*e = *--(*S).top;
	return OK;
}
 
Status StackTraverse(SqStack S , Status(*visit)(SElemType)){
	SElemType *p;
	p = S.base;
	while(S.top > p){
		visit(*p++);
	}
	printf("\n");
	return OK;
}
```

SqStack_main.c
```c
#include "header.c"
typedef int SElemType;
#include "SqStack.c"
Status visit(SElemType c){
	printf("%d ",c);
	return OK;
}
 
void main(){
	int j;
	SqStack s;
	SElemType e;
	if(InitStack(&s) == OK){
		for(j = 1;j<=12;j++){
			Push(&s,j);
		}
	}
	printf("The elements of stack is:");
	StackTraverse(s,visit);
	Pop(&s , &e);
	printf("The top element be poped e=%d\n",e);
	printf("Is stack empty:%d (1:yes 0:no)\n",StackEmpty(s));
	ClearStack(&s);
	printf("After clear stack , is stack empty:%d (1:yes 0:no)\n",StackEmpty(s));
	DestroyStack(&s);
	printf("After destroy stack,s.top = %u s.base = %u s.stacksize = %d\n",s.top,s.base,s.stacksize);
}
```