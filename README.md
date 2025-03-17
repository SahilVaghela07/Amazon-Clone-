# Amazon-Clone-
 This project is a clone of Amazon’s homepage, developed using HTML and CSS to replicate its layout and design. The goal was to enhance my front-end development skills by working on a real-world UI/UX structure. This project focuses on visual presentation and responsiveness without JavaScript functionality. 

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_BOOKS 100
#define MAX_BORROWERS 100

// Structure to store book information
typedef struct {
    int id;
    char title[50];
    char author[50];
    int isAvailable; 
} Book;

// Structure to store borrower information
typedef struct {
    int bookId;  
    char borrowerName[50];
} Borrower;

// Global arrays to hold books and borrowers
Book books[MAX_BOOKS];
Borrower borrowers[MAX_BORROWERS];
int bookCount = 0;
int borrowerCount = 0;


// Function to add a book
void addBook() {
    if (bookCount >= MAX_BOOKS) {
        printf("Library is full!\n");
        return;
    }
    
    Book newBook;
    newBook.id = bookCount + 1; 
    printf("Enter book title: ");
    fgets(newBook.title, sizeof(newBook.title), stdin);
    newBook.title[strcspn(newBook.title, "\n")] = 0;  
    printf("Enter book author: ");
    fgets(newBook.author, sizeof(newBook.author), stdin);
    newBook.author[strcspn(newBook.author, "\n")] = 0;
    
    newBook.isAvailable = 1; 
    books[bookCount++] = newBook;
    printf("Book added successfully!\n");
}


// Function to display all books
void displayBooks() {
    if (bookCount == 0) {
        printf("No books available in the library.\n");
        return;
    }
    
    printf("\nBooks in the library:\n");
    for (int i = 0; i < bookCount; i++) {
        printf("ID: %d, Title: %s, Author: %s, Status: %s\n", books[i].id, books[i].title, books[i].author, 
               books[i].isAvailable ? "Available" : "Borrowed");
    }
}


// Function to borrow a book
void borrowBook() {
    int bookId;
    char borrowerName[50];
    
    printf("Enter your name: ");
    fgets(borrowerName, sizeof(borrowerName), stdin);
    borrowerName[strcspn(borrowerName, "\n")] = 0; 
    
    displayBooks();
    printf("Enter the ID of the book you want to borrow: ");
    scanf("%d", &bookId);
    getchar();  
    
    
    // Check if the book exists and is available
    if (bookId <= 0 || bookId > bookCount) {
        printf("Invalid book ID!\n");
        return;
    }
    
    if (books[bookId - 1].isAvailable == 0) {
        printf("Sorry, this book is already borrowed.\n");
        return;
    }
    
    
    // Borrow the book
    books[bookId - 1].isAvailable = 0;  // Mark the book as borrowed
    Borrower newBorrower;
    newBorrower.bookId = bookId;
    strcpy(newBorrower.borrowerName, borrowerName);
    borrowers[borrowerCount++] = newBorrower;
    
    printf("Book borrowed successfully by %s!\n", borrowerName);
}



// Function to return a borrowed book
void returnBook() {
    int bookId;
    char borrowerName[50];
    
    printf("Enter your name: ");
    fgets(borrowerName, sizeof(borrowerName), stdin);
    borrowerName[strcspn(borrowerName, "\n")] = 0; 
    
    printf("Enter the ID of the book you want to return: ");
    scanf("%d", &bookId);
    getchar();  
    
    // Check if the book is borrowed 
    int found = 0;
    for (int i = 0; i < borrowerCount; i++) {
        if (borrowers[i].bookId == bookId && strcmp(borrowers[i].borrowerName, borrowerName) == 0) {
            
            books[bookId - 1].isAvailable = 1; 
            
            for (int j = i; j < borrowerCount - 1; j++) {
                borrowers[j] = borrowers[j + 1];
            }
            borrowerCount--;
            printf("Book returned successfully!\n");
            found = 1;
            break;
        }
    }
    
    if (!found) {
        printf("No record found for this borrower and book combination.\n");
    }
}


// Function to display the menu
void displayMenu() {
    printf("\nLibrary Management System\n");
    printf("1. Add Book\n");
    printf("2. Display Books\n");
    printf("3. Borrow Book\n");
    printf("4. Return Book\n");
    printf("5. Exit\n");
}

int main() {
    int choice;
    
    while (1) {
        displayMenu();
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar();  
        
        switch (choice) {
            case 1:
                addBook();
                break;
            case 2:
                displayBooks();
                break;
            case 3:
                borrowBook();
                break;
            case 4:
                returnBook();
                break;
            case 5:
                printf("Exiting...\n");
                exit(0);
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }
    
    return 0;
}
