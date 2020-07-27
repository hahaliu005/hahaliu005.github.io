---
title: The Operate Of Stack
tags: 
  - C
  - DataStructure
date: 2018-08-06
---

About Operate Of Stack 

<!-- more -->

```C
typedef char SElemType;
#include "header.c"
#include "SqStack.c"
SElemType Precede(SElemType t1 , SElemType t2){
	SElemType f;
	switch(t2){
		case '+':
		case '-':if(t1 == '(' || t1 == '#'){
			f = '<';
		}else{
			f = '>';
		}
		break;
		case '*':
		case '/':if(t1 == '*' || t1 == '/' || t1 == ')'){
			f = '>';
		}else{
			f = '<';
		}
		break;
		case '(':if(t1==')'){
			printf("ERROR1\n");
			exit(ERROR);
		}else{
			f = '<';
		}
		case ')':switch(t1){
				case '(':f='=';
				break;
				case '#':printf("ERROR2\n");
				exit(ERROR);
				default:f='>';
			}
			break;
		case '#':switch(t1){
				case '#':f='=';
					break;
				case '(':printf("ERROR3\n");
					exit(ERROR);
				default:f='>';
			}
	}
	return f;
}
 
Status In (SElemType c){
	switch(c){
		case '+':
		case '-':
		case '*':
		case '/':
		case '(':
		case ')':
		case '#':return TRUE;
		default:return FALSE;
	}
}
 
SElemType Operate(SElemType a , SElemType theta,SElemType b){
	SElemType c;
	a = a-48;
	b = b-48;
	switch(theta){
		case '+':c = a+b+48;
		break;
		case '-':c = a-b+48;
		break;
		case '*':c = a*b+48;
		break;
		case '/':c = a/b+48;
		break;
	}
	return c;
}
 
SElemType EvaluateExpression(){
	SqStack OPTR,OPND;
	SElemType a,b,c,x,theta;
	InitStack(&OPTR);
	Push(&OPTR,'#');
	InitStack(&OPND);
	c = getchar();
	GetTop(OPTR,&x);
	while(c!='#' || x!='#'){
		if(In(c)){
			switch(Precede(x,c)){
				case '<':Push(&OPTR,c);
				c = getchar();
				break;
				case '=':Pop(&OPTR,&x);
				c = getchar();
				break;
				case '>':Pop(&OPTR,&theta);
				Pop(&OPND,&b);
				Pop(&OPND,&a);
				Push(&OPND,Operate(a,theta,b));
				break;
			}
		}
		else if(c >= '0' && c <= '9'){
			Push(&OPND , c);
			c=getchar();
		}
		else{
			printf("ERROR4\n");
			exit(ERROR);
		}
		GetTop(OPTR,&x);
	}
	GetTop(OPND,&x);
	return x;
}
 
void main(){
	printf("Please enter the value or result at last must between 0~9 , and finish as '#'\n");
	printf("%c\n",EvaluateExpression());
}
```