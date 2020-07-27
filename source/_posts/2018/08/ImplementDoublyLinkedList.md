---
title: Implement Doubly Linked List
tags:
  - C
  - DataStructure
date: 2018-08-06
---

Gist about implementation of doubly linked list.

<!-- more -->

DuLinkList.c
```c
typedef struct DuLNode{
	ElemType data;
	struct DuLNode *prior , *next;
}DuLNode,*DuLinkList;
Status InitList(DuLinkList *L){
	*L = (DuLinkList)malloc(sizeof(DuLNode));
	if(*L){
		(*L)->next = (*L)->prior = (*L);
		return OK;
	}
	else{
		return OVERFLOW;
	}
}
 
Status DestroyList(DuLinkList *L){
	DuLinkList q , p = (*L)->next;
	while(p != *L){
		q = p->next;
		free(p);
		p = q;
	}
	free(*L);
	*L = NULL;
	return OK;
}
 
Status ClearList(DuLinkList L){
	DuLinkList q , p = L->next;
	while(p != L){
		q = p->next;
		free(p);
		p = q;
	}
	L->next = L->prior = L;
	return OK;
}
 
Status ListEmpty(DuLinkList L){
	if(L->next == L && L->prior == L){
		return TRUE;
	}
	else{
		return FALSE;
	}
}
 
int ListLength(DuLinkList L){
	int i = 0;
	DuLinkList p = L->next;
	while(p != L){
		i++;
		p = p->next;
	}
	return i;
}
 
Status GetElem(DuLinkList L , int i , ElemType *e){
	int j = 1;
	DuLinkList p = L->next;
	while(p != L && j < i){
		j++;
		p = p->next;
	}
	if(p == L || j > i){
		return ERROR;
	}else{
		*e = p->data;
		return OK;
	}
}
 
int LocateElem(DuLinkList L , ElemType e , Status(*compare)(ElemType , ElemType)){
	int i = 0;
	DuLinkList p = L->next;
	while(p != L){
		i++;
		if(compare(e , p->data)){
			return i;
		}
		p = p->next;
	}
	return 0;
}
 
Status PriorElem(DuLinkList L , ElemType cur_e , ElemType *pre_e){
	DuLinkList p = L->next->next;
	while(p != L){
		if(p->data == cur_e){
			*pre_e = p->prior->data;
			return OK;
		}
		p = p->next;
	}
	return FALSE;
}
 
Status NextElem(DuLinkList L , ElemType cur_e , ElemType *next_e){
	DuLinkList p = L->next->next;
	while(p != L){
		if(p->prior->data == cur_e){
			*next_e = p->data;
			return TRUE;
		}
		p = p->next;
	}
	return FALSE;
}
 
DuLinkList GetElemP(DuLinkList L , int i){
	int j;
	DuLinkList p = L;
	for(j = 1;j<=i;j++){
		p = p->next;
	}
	return p;
}
 
Status ListInsert(DuLinkList L , int i , ElemType e){
	DuLinkList p , s;
	if( i < 1 || i > ListLength(L) + 1){
		return ERROR;
	}
	p = GetElemP(L , i - 1);
	if(! p){
		return ERROR;
	}
	s = (DuLinkList)malloc(sizeof(DuLNode));
	s->data = e;
	s->prior = p;
	s->next = p->next;
	p->next->prior = s;
	p->next = s;
	return OK;
}
 
Status ListDelete(DuLinkList L , int i , ElemType *e){
	DuLinkList p;
	if(i < 1 || i > ListLength(L)){
		return ERROR;
	}
	p = GetElemP(L , i);
	if(!p){
		return ERROR;
	}
	*e = p->data;
	p->prior->next = p->next;
	p->next->prior = p->prior;
	free(p);
	return OK;
	
}
 
void ListTraverse(DuLinkList L , void(* visit)(ElemType)){
	DuLinkList p = L->next;
	while(p != L){
		visit(p->data);
		p = p->next;
	}
	printf("\n");
}
 
void ListTraverseBack(DuLinkList L , void(*visit)(ElemType)){
	DuLinkList p = L->prior;
	while(p != L){
		visit(p->data);
		p = p->prior;
	}
	printf("\n");
}
```

DuLinkList_main.c
```c
 #include"header.c"
 typedef int ElemType;
 #include"DuLinkList.c"
 
 Status compare(ElemType c1,ElemType c2) 
 {
   if(c1==c2)
     return TRUE;
   else
     return FALSE;
 }
 
 void vd(ElemType c) 
 {
   printf("%d ",c);
 }
 
 void main()
 {
   DuLinkList L;
   int i,n;
   Status j;
   ElemType e;
   InitList(&L);
   for(i=1;i<=5;i++)
     ListInsert(L,i,i); 
   printf("positive squence output list:");
   ListTraverse(L,vd);
   printf("reversed squence output list:");
   ListTraverseBack(L,vd);
   n=2;
   ListDelete(L,n,&e);
   printf("Delete the %d node value is %d , the rest are:",n,e);
   ListTraverse(L,vd);
   printf("The number of list elements are %d\n",ListLength(L));
   printf("Is the list empty:%d (1:yes 0: no) \n ",ListEmpty(L));
   ClearList(L);
   printf("After clear list , is list empty:%d (1:yes 0:no)\n",ListEmpty(L));
   for(i=1;i<=5;i++)
     ListInsert(L,i,i);
   ListTraverse(L,vd);
   n=3;
   j=GetElem(L,n,&e);
   if(j)
     printf("The %d element is %d:\n",n,e);
   else
     printf("There are no %d element.\n",n);
   n=4;
   i=LocateElem(L,n,compare);
   if(i)
     printf("The element which are equal to %d is the %d element\n",n,i);
   else
     printf("There are no element equal to %d\n",n);
   j=PriorElem(L,n,&e);
   if(j)
     printf("%d's previous element is %d \n",n,e);
   else
     printf("There are no %d's previous element\n",n);
   j=NextElem(L,n,&e);
   if(j)
     printf("%d's next element is %d\n",n,e);
   else
     printf("There are no %d's next element\n",n);
   DestroyList(&L);
 }
```