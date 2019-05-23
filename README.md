//# bt2.-Cay-nhi-phan-tim-kiem
//Bai Tap 2. Ngo Quy Huy
#include<iostream>
#include<stdlib.h>
#include<stdio.h>
using namespace std;
typedef int item; //kieu item la kieu nguyen
struct Node
{
     item key; //truong key cua du lieu
     Node *Left, *Right; //con trai va con phai
};
typedef Node *Tree;  //cay
  
int insertNode(Tree &T, item x) // chen 1 Node vao cay
{
    if (T != NULL)
    {
        if (T->key == x) return -1;  // Node nay da co
        if (T->key > x) return insertNode(T->Left, x); // chen vao Node trai
        else if (T->key < x) return insertNode(T->Right, x);   // chen vao Node phai
    }
    T = (Node *) malloc(sizeof(Node));
    if (T == NULL) return 0;    // khong du bo nho
    T->key = x;
    T->Left = T->Right = NULL;
    return 1;   // ok
}
  
void CreateTree(Tree &T)        // nhap cay
{
    int x;
    while (1)
    {
        printf("Nhap vao Node,(0=huy)): ");
        scanf("%d", &x);
        if (x == 0) break;  // x = 0 thi thoat
        int check = insertNode(T, x);
        if (check == -1) printf("Node da ton tai!\n");
        else if (check == 0) printf("Khong du bo nho");
  
    }
}


/////////////////////////////////////////////////////////// 
//Cac cach duyet
 
// Duyet theo LNR
void LNR(Tree T)
{
     if(T!=NULL)
     {
      LNR(T->Left);
      printf("%d \t",T->key);
      LNR(T->Right);
     }
}
  // duyet LRN
void LRN(Tree T)
{
	if (T != NULL)
	{
		LRN(T->Left);
		LRN(T->Right);
		printf("%d \t", T->key);
	}
}
//duyet NLR
void NLR(Tree T)
{
	if (T != NULL)
	{
		printf("%d \t", T->key);
		NLR(T->Left);
		NLR(T->Right);
	}
}


/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//Tim kiem va xoa

Node* searchKey(Tree T, item x)     // tim nut co key x
{
    if (T!=NULL)
    {
        if (T->key == x) { Node *P = T; return P;}
        if (T->key > x) return searchKey(T->Left, x);
        if (T->key < x) return searchKey(T->Right, x);
    }
    return NULL;
}
  
int delKey(Tree &T, item x)     // xoa nut co key x
{
    if (T==NULL) return 0;
    else if (T->key > x) return delKey(T->Left, x);
    else if (T->key < x) return delKey(T->Right, x);
    else // T->key == x
    {
        if (T->Left == NULL) T = T->Right;    // Node chi co cay con phai
        else if (T->Right == NULL) T = T->Left;   // Node chi co cay con trai
        else // Node co ca 2 con
        {
            Node *Q = T->Left;
            while (Q->Right != NULL)
            {
                Q = Q->Right;
            }
            T->key = Q->key;
            delKey(T->Left, Q->key);
        }
    }
    return 1;
}
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//kiem tra 2 con
int kt=0;
Tree Child(Tree T)
{
if(T != NULL)
	if(T->Left != NULL && T->Right != NULL)
		kt=1;			
}

//kiem tra cay rong
int EmptyTree(Tree T)
{
 return T==NULL;
}


//tong so node
int Allnodes(Tree T)
{
 if(EmptyTree(T))
	return 0;
 else   return Allnodes(T->Left)+Allnodes(T->Right)+1;
}
  
/////////////////////////////////////////////////////////
//kiem tra

//kiem tra so nguyen to
bool CheckPrime(int x)
{
	if(x<2) return false;
	else if(x==2) return true;
	else if(x%2==0) return false;
	else if(x>2)
	{
		for(int i=2; i<x; i++)
			if(x%i==0) return false;
	}
	return true;
}

//kiem tra so chan
bool CheckEven(int x)
{
	if(x=0) return false;
	else if(x%2==0) return true;
}


//kiem tra so le
bool CheckOdd(int x)
{
	if(x=0) return false;
	else if(x%2!=0) return true;
}

//////////////////////////////////////////////////////
//tim va tinh tong

int dem=0;
//tong so luong so nguyen to va cac so nguyen to
void SumPrime(Tree T)
{
	if (T != NULL)
	{
		if(CheckPrime(T->key)==true)
		{
			dem++;
			printf("%d \t", T->key);	
		}
		SumPrime(T->Left);
		SumPrime(T->Right);
	}
}

//tong cac so chan
void SumEven(Tree T)
{	
		if (T != NULL)
	{
		if(CheckEven(T->key)==true)
			dem++;		
		SumEven(T->Left);
		SumEven(T->Right);
	}
}

//tong cac so le
void SumOdd(Tree T)
{
		if (T != NULL)
	{
		if(CheckOdd(T->key)==true)
			dem++;		
		SumOdd(T->Left);
		SumOdd(T->Right);
	}
}

//////////////////////////////////////////////////////////////////////////////////////////////////////////
int main()
{
    Tree T;
    T=NULL; //Tao cay rong
  int choice,option;
    bool found;
 
    printf("CHUONG TRINH TEST THU CAY NHI PHAN TIM KIEM\n");
    printf("===========================================");
	printf("\n\n\n");
    printf("1:Tao cay\n");
    printf("2:Tim va Xoa phan tu khoi cay \n");
    printf("3:In ra cac phan tu theo kieu duyet\n");
    printf("4:So nut co 2 con trong cay\n");
    printf("5:So luong va so nguyen to trong cay\n");
    printf("6:Tong so chan va le trong cay\n");
    printf("0:Thoat khoi ung dung\n\n");
 	int i=0;
    while(1)
    {
		int choice;
        printf("Nhap vao lua chon cua ban:");
        scanf("%d", &choice);
        switch(choice)
        {
        case 1: 
                CreateTree(T);
                break;
 
        case 2: Node *P;
    		item x;
    		printf("Nhap vao key can tim: ");
    		scanf("%d", &x);
    		P = searchKey(T, x);
    		if (P != NULL) printf("Tim thay key %d\n", P->key);
    		else printf("Key khong co trong cay %d\n", x);
    		if (delKey(T, x)) printf("Xoa thanh cong\n");
    		
    		else printf("Khong tim thay key %d can xoa\n", x);
                break;
   
            case 3: 
            	printf("\n");
            	printf("Duyet cay theo LNR: \n");
    			LNR(T);
    			printf("\n");
   				printf("Duyet cay theo LRN: \n");
    			LRN(T);
    			printf("\n");
    			printf("Duyet cay theo NRL: \n");
    			NLR(T);
    			printf("\n");   
    			break;
    		case 4:
				for(i;i<Allnodes(T);i++)
				{
					Child(T);
					if (kt==1) i++;	
				}
				printf("\n");
				printf("So nut co 2 con: %d\n", i);	
				printf("\n");					
                break; 
       		case 5:   
			    printf("\n");  			
       			printf("Cac so nguyen to la:\n");     			
       			SumPrime(T);
       			printf("\n");
       			printf("Tong so nguyen to la: %d\n", dem);
       			printf("\n");
       			break;
       		case 6:		
       			SumEven(T);
       			printf("\n");
       			printf("Tong so chan la: %d\n", dem);
       			SumOdd(T);   			
       			printf("Tong so le la: %d\n", dem);
       			printf("\n");
       			break;
        	case 0: return 0;
   
        }
    }
    
}
