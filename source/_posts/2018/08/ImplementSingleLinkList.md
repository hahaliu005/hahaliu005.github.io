---
title: Implement Single Link List
tags:
  - C
  - DataStructure
date: 2018-08-05
---

Gist for implement single link list

<!-- more -->

LinkList.c
```c
typedef struct LNode{
	ElemType data;
	struct LNode *next;
}*Link , *Position , LNode;
typedef struct {
	Link head , tail;
	int len;
}LinkList;
 
Status MakeNode(Link *p , ElemType e){
	*p = (Link)malloc(sizeof(LNode));
	if(!*p){
		return ERROR;
	}
	(*p)->data = e;
	return OK;
}
 
void FreeNode(Link *p){
	free(*p);
	*p = NULL;
}
 
Status InitList(LinkList *L){
	Link p;
	p = (Link)malloc(sizeof(LNode));
	if(p){
		p->next = NULL;
		(*L).head = (*L).tail = p;
		(*L).len = 0;
		return OK;
	}else{
		return ERROR;
	}
}
 
Status ClearList(LinkList *L){
	Link p , q;
	if((*L).head != (*L).tail){
		p = q = (*L).head->next;
		(*L).head->next = NULL;
		while(p != (*L).tail){
			p = q->next;
			free(q);
			q = p;
		}
		free(q);
		(*L).tail = (*L).head;
		(*L).len = 0;
	}
	return OK;
}
 
Status DestroyList(LinkList *L){
	ClearList(L);
	FreeNode(&(*L).head);
	(*L).tail = NULL;
	(*L).len = 0;
	return OK;
}
 
Status InsFirst(LinkList *L , Link h , Link s){
	s->next = h->next;
	h->next = s;
	if(h == (*L).tail){
		(*L).tail = h->next;
	}
	(*L).len++;
	return OK;
}
 
Status DelFirst(LinkList *L , Link h , Link *q){
	*q = h->next;
	if(*q){
		h->next = (*q)->next;
		if(!h->next){
			(*L).tail = h;
		}
		(*L).len--;
		return OK;
	}else{
		return FALSE;
	}
}
 
Status Append(LinkList *L , Link s){
	int i = 1;
	(*L).tail->next = s;
	while(s->next){
		s = s->next;
		i++;
	}
	(*L).tail = s;
	(*L).len += i;
	return OK;
}
 
Position PriorPos(LinkList L , Link p){
	Link q;
	q = L.head->next;
	if(q == p){
		return NULL;
	}else{
		while(q->next != p){
			q = q->next;
		}
		return q;
	}
}
 
Status Remove(LinkList *L , Link *q){
	Link p = (*L).head;
	if((*L).len == 0){
		*q = NULL;
		return FALSE;
	}else{
		while(p->next != (*L).tail){
			p = p->next;
		}
		*q = (*L).tail;
		p->next = NULL;
		(*L).tail = p;
		(*L).len--;
		return OK;
	}
}
 
Status InsBefore(LinkList *L , Link *p , Link s){
	Link q;
	q = PriorPos(*L , *p);
	if(!q){
		q = (*L).head;
	}
	s->next = *p;
	q->next = s;
	*p = s;
	(*L).len++;
	return OK;
}
 
Status InsAfter(LinkList *L , Link *p , Link s){
	if(*p == (*L).tail){
		(*L).tail = s;
	}
	s->next = (*p)->next;
	(*p)->next = s;
	(*p) = s;
	(*L).len++;
	return OK;
}
 
Status SetCurElem(Link p , ElemType e){
	p->data = e;
	return OK;
}
 
ElemType GetCurElem(Link p){
	return p->data;
}
 
Status ListEmpty(LinkList L){
	return L.len ? FALSE : TRUE;
}
 
int ListLength(LinkList L){
	return L.len;
}
 
Position GetHead(LinkList L){
	return L.head;
}
 
Position GetLast(LinkList L){
	return L.tail;
}
 
Position NextPos(Link p){
	return p->next;
}
 
Status LocatePos(LinkList L , int i , Link *p){
	int j;
	*p = L.head;
	for(j = 1;j<=i;j++){
		*p = (*p)->next;
	}
	return OK;
}
 
Position LocateElem(LinkList L , ElemType e , Status(*compare)(ElemType , ElemType)){
	Link p = L.head;
	do{
		p = p->next;
	}
	while(p && !compare(p->data , e));
	return p;
}
 
Status ListTraverse(LinkList L , void(*visit)(ElemType)){
	Link p = L.head->next;
	while(p){
		visit(p->data);
		p = p->next;
	}
	printf("\n");
	return OK;
}
 
Status OrderInsert(LinkList *L,ElemType e,int (*comp)(ElemType,ElemType))
 { 
   Link o,p,q;
   q=(*L).head;
   p=q->next;
   while(p!=NULL&&comp(p->data,e)<0) 
   {
     q=p;
     p=p->next;
   }
   o=(Link)malloc(sizeof(LNode)); 
   o->data=e; 
   q->next=o; 
   o->next=p;
   (*L).len++; 
   if(!p) 
     (*L).tail=o; 
   return OK;
 }
 
Status LocateElemP(LinkList L,ElemType e,Position *q,int(*compare)(ElemType,ElemType))
 {
   Link p=L.head,pp;
   do
   {
     pp=p;
     p=p->next;
   }while(p&&(compare(p->data,e)<0)); 
   if(!p||compare(p->data,e)>0)
   {
     *q=pp;
     return FALSE;
   }
   else 
   {
     *q=p;
     return TRUE;
   }
 }
```

LinkList_main.c
```c
#include "header.c"
typedef int ElemType;
#include "LinkList.c"
Status compare(ElemType c1,ElemType c2) 
 {
   if(c1==c2)
     return TRUE;
   else
     return FALSE;
 }
 
 int cmp(ElemType a,ElemType b)
 { 
   if(a==b)
     return 0;
   else
     return (a-b)/abs(a-b);
 }
 
 void visit(ElemType c)
 {
   printf("%d ",c);
 }
 
 void main()
 {
   Link p,h;
   LinkList L;
   Status i;
   int j,k;
   i=InitList(&L);
   if(!i) 
     exit(FALSE); 
   for(j=1;j<=2;j++)
   {
     MakeNode(&p,j); 
     InsFirst(&L,L.tail,p);
   }
   OrderInsert(&L,0,cmp); 
   for(j=0;j<=3;j++)
   {
     i=LocateElemP(L,j,&p,cmp);
     if(i)
       printf("There have element which value is %d \n",p->data);
     else
       printf("There are no element which value is %d \nThere haven't the element which the value equal to %d\n",j);
   }
   printf("Output Link");
   ListTraverse(L,visit); 
   for(j=1;j<=4;j++)
   {
     printf("delete head node");
     DelFirst(&L,L.head,&p);
     if(p)
       printf("%d\n",GetCurElem(p));
     else
       printf("empty List , can't delete p=%u\n",p);
   }
   printf("the node of list L = %d is L empty %d(1:yes 0:no)\n",ListLength(L),ListEmpty(L));
   MakeNode(&p,10);
   p->next=NULL;
   for(j=4;j>=1;j--)
   {
     MakeNode(&h,j*2);
     h->next=p;
     p=h;
   } 
   Append(&L,h); 
   OrderInsert(&L,12,cmp); 
   OrderInsert(&L,7,cmp); 
   printf("Output List");
   ListTraverse(L,visit); 
   for(j=1;j<=2;j++)
   {
     p=LocateElem(L,j*5,compare);
     if(p)
       printf("There have node %d in List L\n",j*5);
     else
       printf("There have not node %d in List L\n",j*5);
   }
   for(j=1;j<=2;j++)
   {
     LocatePos(L,j,&p); 
     h=PriorPos(L,p); 
     if(h)
       printf("%d's previous element is%d.\n",p->data,h->data);
     else
       printf("%dhave no previous element\n",p->data);
   }
   k=ListLength(L);
   for(j=k-1;j<=k;j++)
   {
     LocatePos(L,j,&p); 
     h=NextPos(p); 
     if(h)
       printf("%d's next element is %d\n",p->data,h->data);
     else
       printf("%d have no next element\n",p->data);
   }
   printf("The element of list L=%d is list L empty %d(1:yes 0:no)\n",ListLength(L),ListEmpty(L));
   p=GetLast(L); 
   SetCurElem(p,15);
   printf("The first element is %d The last element is %d\n",GetCurElem(GetHead(L)->next),GetCurElem(p));
   MakeNode(&h,10);
   InsBefore(&L,&p,h); 
   p=p->next; 
   MakeNode(&h,20);
   InsAfter(&L,&p,h);
   k=ListLength(L);
   printf("Delete the tail node loopy and output:");
   for(j=0;j<=k;j++)
   {
     i=Remove(&L,&p);
     if(!i) 
       printf("Delete failed p=%u\n",p);
     else
       printf("%d ",p->data);
   }
   MakeNode(&p,29); 
   InsFirst(&L,L.head,p);
   DestroyList(&L);
   printf("After destroy listL : L.head=%u L.tail=%u L.len=%d\n",L.head,L.tail,L.len);
 }
```