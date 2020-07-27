---
title: Implement The Linear List
tags:
  - C
  - DataStructure
date: 2018-08-06
---

About the implementation of the linear list.
<!-- more -->

header.c
```c
#include <string.h>
#include <ctype.h>
#include <malloc.h>
#include <limits.h>
#include <stdio.h>
#include <io.h>
#include <math.h>
#include <process.h>
 
#define TRUE 1
#define FALSE 0
#define OK 1
#define ERROR 0
#define INFEASIBLE -1
 
typedef int Status;
typedef int Boolean;
typedef int ElemType;
```

sqList.c
```c
#define LIST_INIT_SIZE 10
#define LISTINCREMENT 2
typedef struct{
	ElemType *elem;
	int length;
	int listSize;
}SqList;
Status InitList(SqList *L){
	(*L).elem = (ElemType *)malloc(LIST_INIT_SIZE*sizeof(ElemType));
	if(!(*L).elem){
		return OVERFLOW;
	}
	(*L).length = 0;
	(*L).listSize = LIST_INIT_SIZE;
	return OK;
}
 
Status DestroyList(SqList *L){
	free((*L).elem);
	(*L).elem = NULL;
	(*L).length = 0;
	(*L).listSize = 0;
	return OK;
}
 
Status ClearList(SqList *L){
	(*L).length = 0;
	return OK;
}
 
Status ListEmpty(SqList L){
	if(L.length == 0){
		return TRUE;
	}else{
		return FALSE;
	}
}
 
int ListLength(SqList L){
	return L.length;
}
 
Status GetElem(SqList L , int i , ElemType *e){
	if(i < 1 || i > L.length){
		return ERROR;
	}
	*e = *(L.elem + i - 1);
	return OK;
}
 
int LocateElem(SqList L , ElemType e , Status(*compare)(ElemType , ElemType)){
	ElemType *p;
	int i = 1;
	p = L.elem;
	while(i <= L.length && !compare(*p++ , e)){
		i++;
	}
	if(i <= L.length){
		return i;
	}else{
		return 0;
	}
}
 
Status PriorElem(SqList L , ElemType cur_e , ElemType * pre_e){
	int i = 2;
	ElemType *p;
	p = L.elem + 1;
	while(i <= L.length && *p != cur_e){
		i ++ ;
		p++;
	}
	if(i <= L.length){
		*pre_e = *--p;
	}else{
		return INFEASIBLE;
	}
	return OK;
}
 
Status NextElem(SqList L, ElemType cur_e , ElemType *next_e){
	int i = 1;
	ElemType *p = L.elem;
	while( i < L.length && *p != cur_e){
		p++;
		i++;
	}
	if(i == L.length){
		return INFEASIBLE;
	}else{
		*next_e = *++p;
		return OK;
	}
	
}
 
Status ListInsert (SqList *L , int i , ElemType e){
	ElemType *newbase , *q , *p;
	if( i < 1 || i > (*L).length + 1){
		return ERROR;
	}
	if((*L).length >= (*L).listSize){
		newbase = (ElemType *)realloc((*L).elem , ((*L).listSize + LISTINCREMENT) * sizeof(ElemType));
		if(!newbase){
			return OVERFLOW;
		}
		(*L).elem = newbase;
		(*L).listSize += LISTINCREMENT;
	}
	q = (*L).elem + i - 1;
	for(p = (*L).elem + (*L).length - 1 ; p >= q ; p--){
		*(p + 1) = *p;
	}
	*q = e ;
	(*L).length++;
	return OK;
}
 
Status ListDelete (SqList *L , int i , ElemType *e){
	ElemType *p , *q;
	if( i < 1 || i > (*L).length){
		return ERROR;
	}
	p = (*L).elem + i - 1;
	*e = *p ;
	q = (*L).elem + (*L).length - 1;
	for(++p ; p <= q ; p++){
		*(p - 1) = *p;
	}
	(*L).length--;
	return OK;
}
 
Status ListTraverse(SqList L, void (*vi)(ElemType *)){
	ElemType *p;
	int i;
	p = L.elem;
	for (i = 1 ; i <= L.length ; i++){
		vi(p++);
	}
	printf("\n");
	return OK;
}
```

sqList_main.c
```c
#include "header.c"
#include "sqList.c"
 
Status comp(ElemType c1 , ElemType c2){
	if(c1 == c2 * c2 ){
		return TRUE;
	}else{
		return FALSE;
	}
}
 
void visit(ElemType *c ) {
	printf("%d " , *c);
}
 
void dbl(ElemType *c){
	*c *= 2;
}
 
 
void main(){
	SqList L;
	ElemType e , e0;
	Status i ;
	int j , k;
	i = InitList(&L);
	printf("after init the list :L.elem = %u L.length = %d L.listsize = %d\n" , L.elem , L.length , L.listSize);
	for(j = 1 ; j <= 5 ; j++){
		i = ListInsert(&L , 1 , j);
	}
	printf("after insert front of the list L value 1~5:*L.elem=");
	for(j = 1 ; j <= 5 ; j++){
		printf("%d " , *(L.elem + j - 1));
	}
	printf("\n");
	printf("L.elem = %u  L.length = %d L.listsize = %d \n" , L.elem , L.length , L.listSize);
	i = ClearList(&L);
	printf("after clear L :L.elem = %u L.length = %d L.listSize = %d \n" , L.elem , L.length , L.listSize);
	i = ListEmpty(L);
	printf("is L empty : i = %d (1:yes 0:no)\n" , i);
	for(j = 1 ; j<= 10 ; j++ ){
		i = ListInsert(&L , j , j);
	}
	printf("after insert at the back of the list L value 1~10 ::*L.elem = ");
	for(j = 1;j <= 10 ; j++){
		printf("%d " , *(L.elem + j - 1));
	}
	printf("\n");
	printf("L.elem = %u L.length = %d L.listSize = %d \n" , L.elem , L.length , L.listSize);
	ListInsert(&L , 1 , 0);
	printf("after insert 0 in the front of the L: * L.elem = ");
	for(j = 1 ; j <= ListLength(L);j++){
		printf("%d " , *(L.elem + j - 1));
	}
	printf("\n");
	printf("L.elem = %u(maybe change) L.length = %d (changed) L.listSize = %d(changed) \n" , L.elem , L.length , L.listSize);
	GetElem(L , 5 , &e);
	printf("the fifth value is:%d\n" , e);
	for(j = 3 ; j <= 4 ; j++ ){
		k = LocateElem(L , j , comp);
		if(k){
			printf("the %d elem's value is %d's double value\n" , k , j);
		}else{
			printf("there is no elem two double of %d\n" , j);
		}
	}
	for(j = 1 ; j <= 2 ; j++ ){
		GetElem(L , j , &e0);
		i = PriorElem(L , e0 , &e);
		if(i == INFEASIBLE){
			printf("elem %d is no prev elem \n" , e0);
		}else{
			printf("elem %d 's prev elem is : %d \n" , e0 , e);
		}
	}
	for(j = ListLength(L) - 1 ; j <= ListLength(L) ; j++){
		GetElem(L , j , &e0);
		i = NextElem(L , e0 , &e);
		if(i == INFEASIBLE) {
			printf("elem %d have not next elem \n" , e0);
		}else{
			printf("elem %d 's next elem is : %d \n" , e0 , e);
		}
	}
	k = ListLength(L);
	for(j = k + 1 ; j >= k ; j-- ) {
		i = ListDelete(&L , j , &e);
		if(i == ERROR){
			printf("failed to delete the %d elem\n" , j);
		}else{
			printf("success to delete the elem's value is : %d \n" , e);
		}
	}
	printf("traver the L and output it :");
	ListTraverse(L , visit);
	printf("with the value doubled : ");
	ListTraverse(L , dbl);
	ListTraverse(L , visit);
	DestroyList(&L);
	printf("after destroy the list : L.elem = %u  L.length = %d L.listSize = %d \n" , L.elem , L.length , L.listSize);
	printf("list test over ");
}
```