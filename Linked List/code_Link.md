# 연결리스트 C언어로 구현 (구조체 포인터 변수)
``` cpp

#include <stdio.h>
#include <iostream>
#include <stdlib.h>
/* 단일 연결리스트 구현 */

// 1. 선언 - 구조체 이용
// 2. 조회 - 인덱스로 활용 될 구조체를 하나 선언하고 헤드로 초기화한 후 값이 NULL이 나올 떄까지 next로 갱신한다.
// 3. 삽입 - 타깃 노드의 다음 노드와 추가할 노드를 연결 시킨 후 타깃 노드를 추가할 노드와 연결시킨다.
// 4. 삭제 - 타깃 노드를 타깃 노드 다음 노드의 다음 노드와 연결시키고 타깃 노드 다음 노드를 메모리 해제

// ** 키 포인트 **

// 삽입 삭제 시 data가 NULL이면 그냥 함수를 종료해버린다.
// 포인터로 선언 시 메모리 할당을 항상 해주도록 하자
// 모든 것이 끝나기 전에 동적으로 할당되었던 메모리는 모두 해제해주자.

typedef struct node{
    int data;
    struct node *next; // 자기 참조 구조체 포인터변수
} Node;

void display(Node *head){
    Node *tmp = head; // 머리 노드 생성
    while(tmp != NULL){
        printf("%d -> ", tmp->data);
        tmp = tmp->next;
    }
    printf("No data\n");
}

void insert(int val, Node *cur){
    Node *New = (Node*)malloc(sizeof(Node)); //삽입할 노드 생성
    New->data = val; // 파라미터로 입력받은 데이터 저장
    New->next = cur->next; // 추가할 노드가 가리키는 것은 cur노드가 가리키고 있는것으로 대입
    cur->next = New; // 현재 가리키고 있는것을 New 노드로 가리킴
}

void insert2(int val, int find, Node *h){ // 삽입 - 찾는 값 뒤에 삽입   h는 시작할 노드를 가져옴
    Node *New = (Node*)malloc(sizeof(Node));
    Node *cur;
    cur = h; // 탐색을 진행할 현재 노드
    New->data = val; // 새 노드에다가 데이터 저장
    New->next = NULL;
    while(cur->next != NULL){
        cur = cur->next;
        if(cur->data == find){
            New->next = cur->next; // 삽입할 새 노드는 현재 노드가 가리키는걸 가리킴
            cur->next = New; // 현재 노드는 삽입할 노드를 가리킴
            break;
        }
    }
}

void Delete(Node *h, int find){
    Node *s;
    Node *p = (Node*)malloc(sizeof(Node));
    p->data = 0;
    p->next = NULL;
    s = h;
    if(s == NULL){
        printf("No\n");
        return ;
    }

    while(s->next != NULL){
        p = s; //p노드를 탐색하기 위한 s노드로 계속 생성
        s = s->next;
        if(s->data == find){
            //메모리 해제시 꼭 free할 위치에 가서 지움
            //지우기전 지울 내용의 전링크에 내용 복사
            p->next = s->next;
            free(s);
            break;
        }
    }
}

int main(){
    Node *node1 = (Node*)malloc(sizeof(Node));
    node1->data = 1;
    node1->next = NULL;

    Node *node2 = (Node*)malloc(sizeof(Node));
    node2->data = 2;
    node2->next = node1->next;
    node1->next = node2;

    Node *node3 = (Node*)malloc(sizeof(Node));
    node3->data = 3;
    node3->next = node2->next;
    node2->next = node3;

    Node *node4 = (Node*)malloc(sizeof(Node));
    node4->data = 4;
    node4->next = node3->next;
    node3->next = node4;

    Node *node5 = (Node*)malloc(sizeof(Node));
    node5->data = 5;
    node5->next = node4->next;
    node4->next = node5;

    display(node1);
    printf("\n");

    insert(6, node3);
    insert2(7, 5, node1);

    display(node1);
    printf("\n");

    Delete(node1, 6);
    Delete(node1, 2);
    display(node1);

    return 0;
}


```

## 출력 결과
<img width="733" alt="스크린샷 2022-02-22 오후 11 27 37" src="https://user-images.githubusercontent.com/86994067/155152388-d2bd0a66-1a89-4043-a130-0e907fdad222.png">

