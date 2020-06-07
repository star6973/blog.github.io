---
layout: post
title: "C Programming [Dynamic Memory and Connection List]"
date: 2014-06-01 17:00:00-18:00:00
categories: Language
tag: Language
---

# 동적메모리와 연결리스트

1. 정적 메모리 
    - 정적(static)  
         : 프로그램이 시작되기 전에 미리 정해진 크기의 메모리를 할당받는 것  
         : 메모리의 크기는 프로그램이 시작하기 전에 결정  
         : 처음에 결정된 크기보다 더 큰 입력이 들어온다면 처리하지 못함  
         : 더 작은 입력이 들어온다면 남은 메모리 공간은 낭비  

2. 동적 메모리
    - 동적(dynamic)
         : 실행 도중에 동적으로 메모리를 할당받는 것   
         : 사용이 끝나면 시스템에 메모리를 반납  
         : 필요한 만큼만 할당을 받고 메모리를 매우 효율적으로 사용  
         : malloc() 계열의 라이브러리 함수 사용(바이트 단위) <stdlib.h>  
         : malloc() 함수의 반환형은 void*이다. 프로그래머가 할당받은 메모리 블록이 어떤 자료형으로 사용할지 알 수 없기 때문에 malloc() 은 효율성을 위하여 동적 메모리를 초기화하지 않는다  
         : 동적 메모리 사용 - 1. * 연산자 2. [] 연산자  

         ex) 

            int *score;
            score = (int*)malloc(100 * sizeof(int));

         : 반납 - free(void *ptr)  

3. 프로그램의 실행 도중에 메모리를 할당받아서 사용하는 것을 동적 메모리라고 한다.

4. 동적으로 메모리를 할당받을 때 사용하는 대표적인 함수는 malloc() 이다.

5. 동적으로 할당된 메모리를 해제하는 함수는 free() 이다.

6. 동적 메모리 할당을 사용하기 위하여 포함시켜야 하는 헤더 파일은 <stdlib.h>이다.

7. 동적 메모리 할당의 응용  
    : 0으로 초기화된 동적 메모리 할당   
    - calloc(size_t n(항목의 개수), size_t size(항목의 크기))  
    - initializer를 먼저 저장하고 할당  
    - 바이트 단위가 아닌 항목 단위로 메모리 할당  
    <br>
    : 재배열  
    - realloc(void *p(기존 동적 메모리), size_t size(새로운 크기))  
    - 할당하였던 메모리 블록의 크기를 변경  

8. 동적 할당 후에 메모리 블록을 초기화하여 넘겨주는 함수는 calloc() 이다.

9. 할당되었던 동적 메모리의 크기를 변경하는 함수는 realloc() 이다.

10. 동적 메모리 할당에서의 단위는 바이트이다.

11. malloc()이 반환하는 자료형은 void *이다.

12. 연결리스트와 배열의 차이점  
    : 배열 

    1. 장점 - 구현이 간단하고 빠르다.
    2. 단점 - 크기가 고정된다. 중간에 삽입, 삭제가 어렵다  .

    : 연결리스트 - 각각의 원소가 포인터를 사용하여 다음 원소의 위치를 가리킨다.
    1. 장점 - 데이터를 저장할 공간이 필요할 때마다 동적으로 공간을 만들어서 쉽게 추가
    2. 단점 - 구현이 어렵고 오류가 나기 쉽다. 중간에 있는 데이터를 빠르게 가져올 수 없다.

            연결 리스트는 줄로 연결된 상자라고 할 수 있다. 
            상자 안에는 데이터가 들어가고 상자에 연결된 줄을 따라가면 다음 상자를 찾을 수 있다. 
            연결 리스트는 일단 데이터를 한군데 모아두는 것을 포기하는 것이다. 
            데이터들은 메인 메모리의 어디에나 흩어져서 존재할 수 있다. 
            그러면 데이터들의 순서를 유지하기 위하여 앞의 데이터는 뒤의 데이터를 가리키는 줄을 가진다. 
            앞의 데이터에서 다음 데이터를 찾아가려면 앞의 데이터의 줄을 따라가면 된다.  
            상자를 연결하는 줄은 포인터로 구현한다

13. 구조 : 노드(node) = 데이터필드(data field) + 링크 필드(link field) // 링크 필드는 다음 원소의 주소를 가리키는 포인터

14. 헤드 포인터 - 첫 번째 노드를 가리키는 포인터

15. 연결 리스트의 이름은 헤드 포인터의 이름

16. 연결 리스트에 노드가 하나도 없으면 헤드 포인터의 값은 NULL이고 이를 공백 연결 리스트라한다.

17. 연결 리스트에서 노드들은 메모리상의 어느 곳에서나 위치할 수 있다. 즉, 노드들의 순서가 리스트상의 순서와 동일하지 않을 수 있다.

18. 자기 참조 구조체 - 구성 멤버 중에서 같은 타입의 구조체를 가리키는 포인터가 존재하는 구조체

    ex1)

        typedef struct NODE {
            int data; // 앞에는 data field
            struct NODE *link; // 자기 참조 구조체, 뒤에는 link field
                               // 현재 구조체를 가리킬 수 있는 포인터
        } NODE;
 
        NODE *p1;
        p1 = (NODE*)malloc(sizeof(NODE)); // 동적 할당
        
        p1 -> 100;
        p1.link = null; // 마지막에는 항상 Null로 끝난다

    ex2)

        #include <stdio.h>
        #include <stdlib.h>
        #include <string.h>
        
        #define S_SIZE 50
        
        typedef struct NODE {
            char title[S_SIZE];
            int year;
            struct NODE *link;
        } NODE;
        
        int main(void)
        {
            NODE *list = NULL; // 헤드 포인터(초기값은 반드시 NULL)
            NODE *prev, *p, *next; // 이전, 현재, 다음
            char buffer[S_SIZE];
            int year;
        
            // 연결 리스트에 정보를 입력한다.
            while(1) {
                printf("책의 제목을 입력하시오:(종료하면 엔터)");
                gets(buffer);
                if(buffer[0] == '\0')
                    break;
        
                p = (NODE *)malloc(sizeof(NODE));
                strcpy(p->title, buffer);
                printf("책의 출판 연도를 입력하시오: ");
                gets(buffer);
                year = atoi(buffer);
                p->year = year;
        
                if(list == NULL)          // 리스트가 비어 있으면
                    list = p;         // 새로운 노드를 첫 번째 노드로 만든다
                else                     // 리스트가 비어 있지 않으면
                    prev->link = p;  // 새로운 노드를 이전 노드의 끝에 붙인다
        
                p->link = NULL;         // 새로운 노드의 링크 필드를 NULL로 설정
                prev = p;
            }
            printf("\n");
        
            // 연결 리스트에 들어 있는 정보를 모두 출력한다
            p = list;
            
            while(p != NULL) {
                printf("책의 제목:%s 출판 연도:%d \n", p->title, p->year);
                p = p->link;
            }
        
            // 동적 할당을 반납한다
            p = list;
            while(p != NULL) {
                next = p->link;
                free(p);
                p = next;
            }
        
            return 0;
        }

19. 연결 리스트에서 다음 노드는 포인터로 가리킨다.

20. 연결 리스트의 일반적인 노드는 데이터 필드와 링크 필드로 구성

21. 구조체의 멤버 중에 자기 자신을 가리키는 포인터가 존재하는 구조체를 자기 참조 구조체라고 한다.

22. 배열은 구현이 간단하고 빠르지만 크기가 정해져 있어서 삽입과 삭제가 어렵다. 연결 리스트는 동적으로 메모리를 할당하여 삽입과 삭제가 쉽지만 구현이 어렵고 오류가 난다.

23. 연결 리스트가 가지고 있는 모든 노드를 한 번씩 차례대로 방문하는 연산을 순회라고 한다.

        NODE *insert_node(NODE *plist, NODE *pprev, DATA item)
        {
        NODE *pnew = NULL;

        if( !(pnew = (NODE *)malloc(sizeof(NODE))) )
        {
            printf("메모리 동적 할당 오류\n");
            exit(1);
        }

        pnew->data = data;
        if( pprev == NULL )	// 연결 리스트의 처음에 삽입
        {
            pnew->link = plist;
            plist = pnew;
        }
        else 		// 연결 리스트의 중간에 삽입
        {
            pnew->link = pprev->link;
            pprev->link = pnew;
        }
        return plist;
        }


        NODE *delete_node(NODE *plist, NODE *pprev, NODE *pcurr)
        {
        if( pprev == NULL )
            plist = pcurr->link;
        else 
            pprev->link = pcurr->link;

        free(pcurr);
        return plist;
        }
