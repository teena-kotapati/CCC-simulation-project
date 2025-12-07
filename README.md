#include <stdio.h>
#include <stdlib.h>
struct Node {
    int data;
    struct Node* next;
};

struct Node* front = NULL;
struct Node* rear = NULL;
int size = 0;

int isEmpty() {
    return front == NULL;
}

void enqueue(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->next = NULL;
    
    if (isEmpty()) {
        front = rear = newNode;
    } else {
        rear->next = newNode;
        rear = newNode;
    }
    size++;
    printf("Parked car #%d (node: %p)\n", data, (void*)newNode);
}

int dequeue() {
    if (isEmpty()) {
        printf(" LINE EMPTY! Nothing to serve.\n\n");
        return -1;
    }
    
    struct Node* temp = front;
    int data = front->data;
    front = front->next;
    
    if (front == NULL) rear = NULL;
    free(temp);
    size--;
    
    printf(" Car #%d left from front (freed: %p)\n", data, (void*)temp);
    return data;
}

void display() {
    if (isEmpty()) {
        printf("Queue is empty\n\n");
        return;
    }
    
    printf("\n=== LINKED LIST QUEUE CHAIN ===\n");
    printf("Size: %d | Front: %p | Rear: %p\n", size, (void*)front, (void*)rear);
    
    struct Node* current = front;
    printf("CHAIN: ");
    while (current != NULL) {
        printf(" [%d|%p]→", current->data, (void*)current->next);
        current = current->next;
    }
    printf(" NULL\n\n");
}

int main() {
    int choice, data;
    printf(" LINKED LIST QUEUE PARKING SIMULATOR\n");
    printf("Dynamic size - NO OVERFLOW LIMIT! Watch nodes link/unlink!\n\n");
    
    while (1) {
        printf(" 1.Park (Enqueue)  2.Leave (Dequeue)   3.Show Chain  4.Quit\n➤ ");
        scanf("%d", &choice);
        if (choice == 4) break;
        
        switch (choice) {
            case 1: 
                printf("Car number? "); scanf("%d", &data); 
                enqueue(data); 
                display(); 
                break;
            case 2: 
                dequeue(); 
                display(); 
                break;
            case 3: 
                display(); 
                break;
            default: printf("Pick 1-4!\n\n");
        }
    }
    
    // Clean up remaining nodes
    while (!isEmpty()) dequeue();
    printf("All nodes freed! Clean exit. \n");
    return 0;
}



