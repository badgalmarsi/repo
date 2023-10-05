# repo

#include <stdio.h>

int main() {
    int choice;
    float balance = 0.0;
    float price;
    
    while (1) {
        printf("Welcome to the Vending Machine!\n");
        printf("1. Cola - $1.50\n");
        printf("2. Chips - $1.00\n");
        printf("3. Candy - $0.75\n");
        printf("4. Exit\n");
        printf("Current Balance: $%.2f\n", balance);
        printf("Enter your choice: ");
        scanf("%d", &choice);
        
        switch (choice) {
            case 1:
                price = 1.50;
                break;
            case 2:
                price = 1.00;
                break;
            case 3:
                price = 0.75;
                break;
            case 4:
                printf("Thank you for using the Vending Machine!\n");
                return 0;
            default:
                printf("Invalid choice. Please try again.\n");
                continue;
                
        
    }
    
return 0;
}
}
