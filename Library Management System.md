#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Structure to represent a book
typedef struct Book {
    int id;
    char title[50];
    char author[50];
    struct Book *next;
} Book;

Book *head = NULL; // Head pointer for the book list

// Function to add a book
void addBook(int id, char title[], char author[]) {
    Book *newBook = (Book *)malloc(sizeof(Book));
    if (!newBook) {
        printf("Memory allocation failed!\n");
        return;
    }
    newBook->id = id;
    strncpy(newBook->title, title, 49);
    newBook->title[49] = '\0';  // Ensure null termination
    strncpy(newBook->author, author, 49);
    newBook->author[49] = '\0'; // Ensure null termination
    newBook->next = head;
    head = newBook;
    printf("Book added successfully!\n");
}

// Function to display books
void displayBooks() {
    Book *temp = head;
    if (!temp) {
        printf("No books available!\n");
        return;
    }
    printf("\nLibrary Books:\n");
    while (temp) {
        printf("ID: %d, Title: %s, Author: %s\n", temp->id, temp->title, temp->author);
        temp = temp->next;
    }
}

// Function to search for a book
void searchBook(int id) {
    Book *temp = head;
    while (temp) {
        if (temp->id == id) {
            printf("Book Found: ID: %d, Title: %s, Author: %s\n", temp->id, temp->title, temp->author);
            return;
        }
        temp = temp->next;
    }
    printf("Book not found!\n");
}

// Function to delete a book
void deleteBook(int id) {
    Book *temp = head, *prev = NULL;
    while (temp && temp->id != id) {
        prev = temp;
        temp = temp->next;
    }
    if (!temp) {
        printf("Book not found!\n");
        return;
    }
    if (!prev)
        head = temp->next;
    else
        prev->next = temp->next;
    free(temp);
    printf("Book deleted successfully!\n");
}

// Function to free memory before exiting
void freeMemory() {
    Book *temp;
    while (head) {
        temp = head;
        head = head->next;
        free(temp);
    }
}

int main() {
    int choice, id;
    char title[50], author[50];

    do {
        printf("\nLibrary Management System\n");
        printf("1. Add Book\n2. Display Books\n3. Search Book\n4. Delete Book\n5. Exit\nEnter choice: ");
        scanf("%d", &choice);
        getchar(); // Consume newline character left by scanf

        switch (choice) {
            case 1:
                printf("Enter ID: ");
                scanf("%d", &id);
                getchar(); // Consume newline character
                printf("Enter Title: ");
                scanf(" %49[^\n]", title);
                printf("Enter Author: ");
                scanf(" %49[^\n]", author);
                addBook(id, title, author);
                break;
            case 2:
                displayBooks();
                break;
            case 3:
                printf("Enter Book ID to search: ");
                scanf("%d", &id);
                searchBook(id);
                break;
            case 4:
                printf("Enter Book ID to delete: ");
                scanf("%d", &id);
                deleteBook(id);
                break;
            case 5:
                freeMemory();
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Try again.\n");
        }
    } while (choice != 5);

    return 0;
}
