---
title: Implement Another Single Link List
tags:
  - C
  - DataStructure
date: 2018-08-05
---

Gist for implement single link list

<!-- more -->

SLinkList.c
```c
struct LNode{
	ElemType data;
	struct LNode *next;
};
typedef struct LNode *LinkList;
Status InitList(LinkList *L){
	*L = (LinkList)malloc(sizeof(struct LNode));
	if(!*L){
		exit(OVERFLOW);
	}
	(*L)->next = NULL;
	return OK;
}
 
Status DestroyList(LinkList *L){
	LinkList q;
	while(*L){
		q = (*L)->next;
		free(*L);
		*L = q;
	}
	return OK;
}
 
Status ClearList(LinkList L){
	LinkList p,q;
	p = L->next;
	while(p){
		q = p->next;
		free(p);
		p = q;
	}
	L->next = NULL;
	return OK;
}
 
Status ListEmpty(LinkList L){
	if(!L->next){
		return TRUE;
	}else{
		return FALSE;
	}
}
 
int ListLength(LinkList L){
	int i = 0;
	LinkList p = L->next;
	while(p){
		i++;
		p = p->next;
	}
	return i;
}
 
Status GetElem(LinkList L , int i , ElemType *e){
	int j = 1;
	LinkList p = L->next;
	while(p && j < i ){
		j++;
		p = p->next;
	}
	if(!p || j > i){
		return ERROR;
	}
	*e = p->data;
	return OK;
}
 
int LocateElem(LinkList L , ElemType e , Status(*compare)(ElemType , ElemType)){
	int i = 0;
	LinkList p = L->next;
	while(p){
		i++;
		if(compare(p->data , e)){
			return i;
		}
		p = p->next;
	}
	return 0;
}
 
Status PriorElem(LinkList L , ElemType cur_e , ElemType *pre_e){
	LinkList p = L->next;
	while(p->next){
		if(p->next->data == cur_e){
			*pre_e = p->data;
			return OK;
		}
		p = p->next;
	}
	return INFEASIBLE;
}
 
Status NextElem(LinkList L , ElemType cur_e , ElemType *next_e){
	LinkList p = L->next;
	while(p->next){
		if(p->data == cur_e){
			*next_e = p->next->data;
			return OK;
		}
		p = p->next;
		
	}
	return INFEASIBLE;
}
 
Status ListInsert(LinkList L , int i , ElemType e){
	LinkList q , p = L;
	int j = 0;
	while(p && j < i - 1 ){
		p = p->next;
		j++;
	}
	if(!p || j > i - 1){
		return ERROR;
	}
	q = (LinkList)malloc(sizeof(struct LNode));
	q->data = e;
	q->next = p->next;
	p->next = q;
	return OK;
}
 
Status ListDelete(LinkList L , int i , ElemType *e){
	LinkList q , p = L;
	int j = 1;
	while(p->next && j < i){
		j++;
		p = p->next;
	}
	if(!p->next || j > i){
		return ERROR;
	}
	q = p->next;
	p->next = q->next;
	*e = q->data;
	free(q);
	return OK;
}
 
Status ListTraverse(LinkList L , void(*vi)(ElemType)){
	LinkList p = L->next;
	while(p){
		vi(p->data);
		p = p->next;
	}
	printf("\n");
	return OK;
}
 
void InsertAscend(LinkList L , ElemType e){
	LinkList p = L , q;
	while(p->next && p->next->data < e){
		p = p->next;
	}
	q = (LinkList)malloc(sizeof(struct LNode));
	q->data = e;
	q->next = p->next;
	p->next = q;
}
 
void InsertDescend(LinkList L , ElemType e){
	LinkList p = L , q;
	while(p->next && p->next->data > e){
		p = p->next;
	}
	q = (LinkList)malloc(sizeof(struct LNode));
	q->data = e;
	q->next = p->next;
	p->next = q;
}
 
Status HeadInsert(LinkList L , ElemType e){
	LinkList p;
	p = (LinkList)malloc(sizeof(struct LNode));
	p->data = e;
	p->next = L->next;
	L->next = p;
	return OK;
}
 
Status EndInsert(LinkList L , ElemType e){
	LinkList p = L , q;
	while(p->next){
		p = p->next;
	}
	q = (LinkList)malloc(sizeof(struct LNode));
	q->data = e;
	q->next = p->next;
	p->next = q;
	return OK;
}
 
Status DeleteFirst(LinkList L , ElemType *e){
	LinkList p = L->next;
	if(!p){
		return ERROR;
	}
	*e = p->data;
	L->next = p->next;
	free(p);
	return OK;
}
 
Status DeleteTail(LinkList L , ElemType *e){
	LinkList p = L;
	if(!p->next){
		return ERROR;
	}
	while(p->next->next){
		p = p->next;
	}
	*e = p->next->data;
	free(p->next);
	p->next = NULL;
	return OK;
}
 
Status DeleteElem(LinkList L , ElemType e){
	LinkList p = L->next , q = L;
	while(p){
		if(p->data == e){
			q->next = p->next;
			free(p);
			return TRUE;
		}else{
			q = p;
			p = p->next;
		}
	}
	return FALSE;
}
 
Status ReplaceElem(LinkList L , int i , ElemType e){
	int j = 1;
	LinkList p = L->next;
	while(p && j < i){
		j++;
		p = p->next;
	}
	if(!p || j > i){
		return ERROR;
	}
	p->data = e;
	return OK;
}
 
Status CreatAscend(LinkList *L , int n){
	int j;
	LinkList p,q,s;
	if(n<=0){
		return ERROR;
	}
	InitList(L);
	printf("Please enter %d elements:\n",n);
	s = (LinkList)malloc(sizeof(struct LNode));
	scanf("%d",&s->data);
	s->next = NULL;
	(*L)->next = s;
	for(j = 1 ; j < n ; j++){
		s = (LinkList)malloc(sizeof(struct LNode));
		scanf("%d",&s->data);
		q = *L;
		p = (*L)->next;
		while(p && p->data < s->data){
			q = p;
			p = p->next;
		}
		s->next = q->next;
		q->next = s;
	}
	return OK;
}
 
Status CreatDescend(LinkList *L , int n){
	int j;
	LinkList p , q , s;
	if(n<=0){
		return ERROR;
	}
	InitList(L);
	printf("Please enter %d elements:\n",n);
	s = (LinkList)malloc(sizeof(struct LNode));
	scanf("%d",&s->data);
	s->next = NULL;
	(*L)->next = s;
	for(j = 1 ; j < n ; j++){
		s = (LinkList)malloc(sizeof(struct LNode));
		scanf("%d",&s->data);
		q = *L;
		p = (*L)->next;
		while(p && p->data > s->data){
			q = p;
			p = p->next;
		}
		s->next = q->next;
		q->next = s;
	}
	return OK;
}
 
Status GetFirstElem(LinkList L , ElemType *e){
	if(!L->next){
		return ERROR;
	}
	*e = L->next->data;
	return OK;
}
```

SLinkList_main.c
```c
#include "header.c"
typedef int ElemType;
#include "SLinkList.c"
Status comp(ElemType c1,ElemType c2)
{
   if(c1==c2)
     return TRUE;
   else
     return FALSE;
 }
 
 void visit(ElemType c)
 {
   printf("%d ",c);
 }
 
 void main()
 {
   LinkList L;
   ElemType e,e0,d;
   Status i;
   int j,k,n;
   i=InitList(&L);
   for(j=1;j<=5;j++)
     i=ListInsert(L,1,j);
   printf("Sequence insert 1~5 at the head of L: L=");
   ListTraverse(L,visit);
   i=ListEmpty(L);
   printf("Is L empty:i=%d(1:yes 0:no)\n",i);
   i=ClearList(L);
   printf("After clear L:L=");
   ListTraverse(L,visit);
   i=ListEmpty(L);
   printf("Is L empty : i=%d(1:yes 0:no)\n",i);
   for(j=1;j<=10;j++)
     ListInsert(L,j,j);
   printf("Sequence insert 1~10 at the tail of L:L=");
   ListTraverse(L,visit);
   GetElem(L,5,&e);
   printf("The 5th element is:%d\n",e);
   for(j=0;j<=1;j++)
   {
     k=LocateElem(L,j,comp);
     if(k)
       printf("The %dthe element is %d\n",k,j);
     else
       printf("There have not element which equal to %d \n",j);
   }
   for(j=1;j<=2;j++)
   {
     GetElem(L,j,&e0);
     i=PriorElem(L,e0,&e);
     if(i==INFEASIBLE)
       printf("The %d have not previous element\n",e0);
     else
       printf("The %d previous element is %d\n",e0,e);
   }
   for(j=ListLength(L)-1;j<=ListLength(L);j++)
   {
     GetElem(L,j,&e0);
     i=NextElem(L,e0,&e);
     if(i==INFEASIBLE)
       printf("The %d have not next element\n",e0);
     else
       printf("The %d next element is %d\n",e0,e);
   }
   k=ListLength(L);
   for(j=k+1;j>=k;j--)
   {
     i=ListDelete(L,j,&e);
     if(i==ERROR)
       printf("Delete the %dth element failed\n",j);
     else
       printf("The element be deleted is %d\n",e);
   }
   printf("Sequence ouput L:");
   ListTraverse(L,visit);
   DestroyList(&L);
   printf("After Destroy L:L=%u\n",L);
   
   printf("For ascend sequence to create n element L,please enter the number of element n:");
   scanf("%d",&n);
   CreatAscend(&L,n);
   printf("Sequence output L:");
   ListTraverse(L,visit);
   InsertAscend(L,10);
   printf("After insert 10 for ascend:the L is:");
   ListTraverse(L,visit);
   HeadInsert(L,12);
   EndInsert(L,9);
   printf("Insert 12 at head , 9 at tail , now L is:");
   ListTraverse(L,visit);
   i=GetFirstElem(L,&e);
   printf("The first element is:%d\n",e);
   printf("Please enter the value you want to delete:");
   scanf("%d",&e);
   i=DeleteElem(L,e);
   if(i)
     printf("Delete %d success!\n",e);
   else
     printf("There have not the value %d!\n",e);
   printf("Now L is:");
   ListTraverse(L,visit);
   printf("Please enter the index of element you want to replace and the new value (index value):");
   scanf("%d%d",&n,&e);
   ReplaceElem(L,n,e);
   printf("Now L is:");
   ListTraverse(L,visit);
   DestroyList(&L);
   printf("After destroy L,create L for descend ,enter the number of element n (n>2)");
   scanf("%d",&n);
   CreatDescend(&L,n);
   printf("Sequence output L:");
   ListTraverse(L,visit);
   InsertDescend(L,10);
   printf("Insert 10 for descend sequence , the L is:");
   ListTraverse(L,visit);
   printf("Enter the value which you want to delete:");
   scanf("%d",&e);
   i=DeleteElem(L,e);
   if(i)
     printf("Delete %d success!\n",e);
   else
     printf("There have not element which equal to %d!\n",e);
   printf("Now L is:");
   ListTraverse(L,visit);
   DeleteFirst(L,&e);
   DeleteTail(L,&d);
   printf("Delete head element %d and tail element %d , L is:",e,d);
   ListTraverse(L,visit);
 }
```