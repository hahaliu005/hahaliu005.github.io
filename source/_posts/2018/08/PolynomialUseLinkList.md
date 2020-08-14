---
title: Polynomial use LinkList
tags:
  - C
  - DataStructure
date: 2018-08-05
---

Gist about polynomial use LinkList 

<!-- more -->

Polynomial.c
```c
#include "LinkList.c"
typedef LinkList polynomial;
#define DestroyPolyn DestroyList
#define PolynLength ListLength
 
Status OrderInsertMerge(LinkList *L , ElemType e , int(*compare)(term , term)){
	Position q , s;
	if(LocateElemP(*L,e,&q,compare)){
		q->data.coef += e.coef;
		if(!q->data.coef){
			s = PriorPos(*L,q);
			if(!s){
				s = (*L).head;
				DelFirst(L,s,&q);
				FreeNode(&q);
			}
		}
		return OK;
	}
	else{
		if(MakeNode(&s,e)){
			InsFirst(L,q,s);
			return OK;
		}
		else{
			return ERROR;
		}
	}
}
 
int cmp(term a , term b){
	if(a.expn == b.expn){
		return 0;
	}
	else{
		return (a.expn - b.expn)/abs(a.expn - b.expn);
	}
}
 
void CreatPolyn(polynomial *P , int m){
	Position q,s;
	term e;
	int i;
	InitList(P);
	printf("Please enter %d coef , expn:\n",m);
	for(i = 1 ; i <= m ; ++i){
		scanf("%f,%d",&e.coef,&e.expn);
		if(!LocateElemP(*P,e,&q,cmp)){
			if(MakeNode(&s,e)){
				InsFirst(P,q,s);
			}
		}
	}
}
 
void PrintPolyn(polynomial P){
	Link q;
	q = P.head->next;
	printf("coef expn\n");
	while(q){
		printf("%f %d\n",q->data.coef,q->data.expn);
		q = q->next;
	}
}
 
void AddPolyn(polynomial *Pa , polynomial *Pb){
	Position ha,hb,qa,qb;
	term a,b;
	ha = GetHead(*Pa);
	hb = GetHead(*Pb);
	qa = NextPos(ha);
	qb = NextPos(hb);
	while(!ListEmpty(*Pa) && !ListEmpty(*Pb) && qa){
		a = GetCurElem(qa);
		b = GetCurElem(qb);
		switch(cmp(a,b)){
			case -1:
				ha = qa;
				qa = NextPos(ha);
				break;
			case 0:
				qa->data.coef += qb->data.coef;
				if(qa->data.coef == 0){
					DelFirst(Pa,ha,&qa);
					FreeNode(&qa);
				}
				else{
					ha = qa;
				}
				DelFirst(Pb , hb , &qb);
				FreeNode(&qb);
				qb = NextPos(hb);
				qa = NextPos(ha);
				break;
			case 1:
				DelFirst(Pb , hb , &qb);
				InsFirst(Pa , ha , qb);
				ha = ha->next;
				qb = NextPos(hb);
		}
	}
	if(!ListEmpty(*Pb)){
		(*Pb).tail = hb;
		Append(Pa , qb);
	}
	DestroyPolyn(Pb);
}
 
void AddPolyn1(polynomial *Pa , polynomial *Pb){
	Position qb;
	term b;
	qb = GetHead(*Pb);
	qb = qb->next;
	while(qb){
		b = GetCurElem(qb);
		OrderInsertMerge(Pa,b,cmp);
		qb = qb->next;
	}
	DestroyPolyn(Pb);
}
 
void Opposite(polynomial Pa){
	Position p;
	p = Pa.head;
	while(p->next){
		p = p->next;
		p->data.coef*=-1;
	}
}
 
void SubtractPolyn(polynomial *Pa , polynomial *Pb){
	Opposite(*Pb);
	AddPolyn(Pa,Pb);
}
 
void MultiplyPolyn(polynomial *Pa , polynomial *Pb){
	polynomial Pc;
	Position qa,qb;
	term a,b,c;
	InitList(&Pc);
	qa = GetHead(*Pa);
	qa = qa->next;
	while(qa){
		a = GetCurElem(qa);
		qb = GetHead(*Pb);
		qb = qb->next;
		while(qb){
			b = GetCurElem(qb);
			c.coef = a.coef * b.coef;
			c.expn = a.expn + b.expn;
			OrderInsertMerge(&Pc , c , cmp);
			qb = qb->next;
		}
		qa = qa->next;
	}
	DestroyPolyn(Pb);
	ClearList(Pa);
	(*Pa).head = Pc.head;
	(*Pa).tail = Pc.tail;
	(*Pa).len = Pc.len;
}
```

Polynomial.c
```c
#include "header.c"
typedef struct{
	float coef;
	int expn;
}term,ElemType;
#include "Polynomial.c"
 
void main(){
	polynomial p,q;
	int m;
	printf("Please enter the number of the first polynomial:");
	scanf("%d",&m);
	CreatPolyn(&p , m);
	printf("Please enter the number of the second polynomial:");
	scanf("%d",&m);
	CreatPolyn(&q , m);
	AddPolyn(&p , &q);
	printf("The result of two polynomial is:\n");
	PrintPolyn(p);
	printf("Please enter the number of the third polynomial:");
	scanf("%d" , &m);
	CreatPolyn(&q,m);
	AddPolyn1(&p , &q);
	printf("The result of two polynomial is(another approche):\n");
	PrintPolyn(p);
	printf("Please enter the number of the fouth polynomial:");
	scanf("%d",&m);
	CreatPolyn(&q,m);
	MultiplyPolyn(&p,&q);
	printf("The result of two polynomial multiply is:\n");
	PrintPolyn(p);
	DestroyPolyn(&p);
}
```
