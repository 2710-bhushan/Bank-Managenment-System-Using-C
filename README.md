**Bank Management System Using C**

## Introduction
A Bank Management System is a software application used to manage banking operations like account creation, deposits, withdrawals, and balance inquiries. This project implements a simple Bank Management System using the C programming language.

## Features
1. **Create an Account**: Allows users to open a new bank account.
2. **Deposit Money**: Users can deposit money into their accounts.
3. **Withdraw Money**: Allows users to withdraw money from their accounts.
4. **Check Balance**: Users can check their account balance.
5. **Close Account**: Users can close their accounts.
6. **Modify Account**: Allows updating user details.

## Implementation
The system stores account details in a file to maintain records even after the program exits.

### Header Files
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
```

### Structure for Account
```c
typedef struct {
    int accountNumber;
    char name[50];
    float balance;
} Account;
```

### Function to Create Account
```c
void createAccount() {
    Account acc;
    FILE *file = fopen("accounts.dat", "ab");
    
    printf("Enter Account Number: ");
    scanf("%d", &acc.accountNumber);
    printf("Enter Name: ");
    scanf("%s", acc.name);
    acc.balance = 0;
    
    fwrite(&acc, sizeof(Account), 1, file);
    fclose(file);
    
    printf("Account Created Successfully!\n");
}
```

### Function to Deposit Money
```c
void depositMoney() {
    FILE *file = fopen("accounts.dat", "rb+"), *temp;
    Account acc;
    int accNo;
    float amount;
    
    printf("Enter Account Number: ");
    scanf("%d", &accNo);
    
    while (fread(&acc, sizeof(Account), 1, file)) {
        if (acc.accountNumber == accNo) {
            printf("Enter Amount to Deposit: ");
            scanf("%f", &amount);
            acc.balance += amount;
            fseek(file, -sizeof(Account), SEEK_CUR);
            fwrite(&acc, sizeof(Account), 1, file);
            fclose(file);
            printf("Amount Deposited Successfully!\n");
            return;
        }
    }
    printf("Account Not Found!\n");
    fclose(file);
}
```

### Function to Withdraw Money
```c
void withdrawMoney() {
    FILE *file = fopen("accounts.dat", "rb+"), *temp;
    Account acc;
    int accNo;
    float amount;
    
    printf("Enter Account Number: ");
    scanf("%d", &accNo);
    
    while (fread(&acc, sizeof(Account), 1, file)) {
        if (acc.accountNumber == accNo) {
            printf("Enter Amount to Withdraw: ");
            scanf("%f", &amount);
            if (amount > acc.balance) {
                printf("Insufficient Balance!\n");
            } else {
                acc.balance -= amount;
                fseek(file, -sizeof(Account), SEEK_CUR);
                fwrite(&acc, sizeof(Account), 1, file);
                printf("Withdrawal Successful!\n");
            }
            fclose(file);
            return;
        }
    }
    printf("Account Not Found!\n");
    fclose(file);
}
```

### Function to Check Balance
```c
void checkBalance() {
    FILE *file = fopen("accounts.dat", "rb");
    Account acc;
    int accNo;
    
    printf("Enter Account Number: ");
    scanf("%d", &accNo);
    
    while (fread(&acc, sizeof(Account), 1, file)) {
        if (acc.accountNumber == accNo) {
            printf("Current Balance: %.2f\n", acc.balance);
            fclose(file);
            return;
        }
    }
    printf("Account Not Found!\n");
    fclose(file);
}
```

### Main Function
```c
int main() {
    int choice;
    while (1) {
        printf("\nBank Management System\n");
        printf("1. Create Account\n");
        printf("2. Deposit Money\n");
        printf("3. Withdraw Money\n");
        printf("4. Check Balance\n");
        printf("5. Exit\n");
        printf("Enter Your Choice: ");
        scanf("%d", &choice);
        
        switch (choice) {
            case 1: createAccount(); break;
            case 2: depositMoney(); break;
            case 3: withdrawMoney(); break;
            case 4: checkBalance(); break;
            case 5: exit(0);
            default: printf("Invalid Choice!\n");
        }
    }
    return 0;
}
```

## Conclusion
This C-based **Bank Management System** provides basic functionalities such as account creation, deposit, withdrawal, and balance inquiry. It uses file handling to store account details permanently. This project serves as a foundation for developing more advanced banking applications with additional features like transaction history, account deletion, and online banking services.

