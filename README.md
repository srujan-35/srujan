#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 50

// Queue for incoming orders
typedef struct {
    int front, rear;
    char orders[MAX][50];
} Queue;

// Stack for prepared meals
typedef struct {
    int top;
    char meals[MAX][50];
} Stack;

// Queue functions
void initQueue(Queue *q) {
    q->front = q->rear = -1;
}

int isQueueFull(Queue *q) {
    return q->rear == MAX - 1;
}

int isQueueEmpty(Queue *q) {
    return q->front == -1;
}

void enqueue(Queue *q, char *item) {
    if (isQueueFull(q)) {
        printf("Order queue is full!
");
        return;
    }
    if (q->front == -1) q->front = 0;
    strcpy(q->orders[++q->rear], item);
    printf("Order added: %s
", item);
}

char* dequeue(Queue *q) {
    if (isQueueEmpty(q)) {
        printf("No pending orders!
");
        return NULL;
    }
    char *item = q->orders[q->front];
    if (q->front == q->rear)
        q->front = q->rear = -1;
    else
        q->front++;
    return item;
}

// Stack functions
void initStack(Stack *s) {
    s->top = -1;
}

int isStackFull(Stack *s) {
    return s->top == MAX - 1;
}

int isStackEmpty(Stack *s) {
    return s->top == -1;
}

void push(Stack *s, char *item) {
    if (isStackFull(s)) {
        printf("Meal stack is full!
");
        return;
    }
    strcpy(s->meals[++s->top], item);
    printf("Meal prepared: %s
", item);
}

void pop(Stack *s) {
    if (isStackEmpty(s)) {
        printf("No prepared meals!
");
        return;
    }
    printf("Meal delivered: %s
", s->meals[s->top--]);
}

// Display functions
void displayQueue(Queue *q) {
    if (isQueueEmpty(q)) {
        printf("No pending orders.
");
        return;
    }
    printf("Pending Orders:
");
    for (int i = q->front; i <= q->rear; i++)
        printf(" - %s
", q->orders[i]);
}

void displayStack(Stack *s) {
    if (isStackEmpty(s)) {
        printf("No prepared meals.
");
        return;
    }
    printf("Prepared Meals:
");
    for (int i = s->top; i >= 0; i--)
        printf(" - %s
", s->meals[i]);
}

int main() {
    Queue q;
    Stack s;
    initQueue(&q);
    initStack(&s);

    int choice;
    char item[50];

    while (1) {
        printf("
--- Cafeteria Order Management ---
");
        printf("1. Add Order
");
        printf("2. Prepare Next Order
");
        printf("3. Deliver Meal
");
        printf("4. Show Pending Orders
");
        printf("5. Show Prepared Meals
");
        printf("6. Exit
");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter order name: ");
                scanf("%s", item);
                enqueue(&q, item);
                break;
            case 2: {
                char *order = dequeue(&q);
                if (order) push(&s, order);
                break;
            }
            case 3:
                pop(&s);
                break;
            case 4:
                displayQueue(&q);
                break;
            case 5:
                displayStack(&s);
                break;
            case 6:
                exit(0);
            default:
                printf("Invalid choice!
");
        }
    }
}
