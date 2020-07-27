---
title: Merge Linear List
tags:
  - C
  - DataStructure
date: 2018-08-06
---

Gist about merge linear list.

<!-- more -->

main.c
```c
#include "header.c"
#include "sqList.c"
 
//algo 2-1
Status MergeList(SqList La , SqList Lb , SqList *Lc){
	int i , j , k = 0 , La_len , Lb_len;
	ElemType *a,*b,ai,bj;
	InitList(Lc);
	La_len = ListLength(La) ;
	Lb_len = ListLength(Lb) ;
	a  = La.elem;
	b = Lb.elem;
	for( i = 1 ; i < La_len ; i++){
		if(*a > *++a){
			return ERROR;
		}
	}
	for( j = 1 ; j < Lb_len ; j++){
		if(*b > *++b){
			return ERROR;
		}
	}
	i = j = 1;
	while( i <= La_len && j <= Lb_len){
		GetElem(La , i , &ai);
		GetElem(Lb , j , &bj);
		if(ai <= bj){
			ListInsert(Lc , ++k , ai);
			++i;
		}else{
			ListInsert(Lc , ++k , bj);
			++j;
		}
	}
	while( i <= La_len){
		GetElem(La , i++ , &ai);
		ListInsert(Lc , ++k , ai);
	}
	while( j <= Lb_len){
		GetElem(Lb , j++ , &bj);
		ListInsert(Lc , ++k , bj);
	}
}
void main(){
	SqList La , Lb , Lc;
	ElemType Na[4] = {3 , 5 , 8 , 11};
	ElemType Nb[7] = {2 , 6 , 8 , 9 , 11 , 15 , 20};
	int  j ;
	Status i ;
	InitList(&La);
	for(j = 0 ; j < 4 ; j++){
		ListInsert(&La , j + 1 , Na[j]);
	}
	InitList(&Lb);
	for(j = 0 ; j < 7 ; j++){
		ListInsert(&Lb , j + 1 , Nb[j]);
	}
	printf("La is:");
	for(j = 1 ; j <= ListLength(La); j++){
		printf("%d " , *(La.elem + j - 1));
	}
	printf("La.length = %d La.listSize = %d " , La.length , La.listSize);
	printf("\n Lb is:");
	for(j = 1 ; j<= ListLength(Lb) ; j++){
		printf("%d " , *(Lb.elem + j - 1));
	}
	printf("Lb.length = %d Lb.listSize = %d " , Lb.length , Lb.listSize);
	printf("\nmerge La and Lb to Lc :\n");
	i = MergeList(La , Lb , &Lc);
	if(i != ERROR){
		printf("now Lc is : ");
		for(j = 1 ; j<=ListLength(Lc) ; j++ ) {
			printf("%d " , *(Lc.elem + j - 1));
		}
	}else{
		printf("the La or Lb is not correct to merge to Lc \n");
	}
}
```

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

