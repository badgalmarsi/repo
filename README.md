#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Coffee Choices
const char cappuccino[] = "Cappuccino";
const char mocha[] = "Mocha";
const char espresso[] = "Espresso";

float coffee_price_A = 3.5f;
float coffee_price_B = 5.5f;
float coffee_price_C = 4.5f;
float coffee_newprice_A;
float coffee_newprice_B;
float coffee_newprice_C;

// Vending machine quantity
int quantity_A = 10;
int quantity_B = 10;
int quantity_C = 10;

int admin_password = 1234;  // Admin password
const int MIN = 5;
float total_amount = 0.0f;

// Ingredient quantities
int coffee_beans = 100;    // Total coffee beans in grams
int water = 500;           // Total water in milliliters
int milk = 300;            // Total milk in milliliters
int chocolate_syrup = 200; // Total chocolate syrup in milliliters

// Function to check ingredient availability
int checkIngredients(int beans_needed, int water_needed, int milk_needed, int syrup_needed) {
    if (coffee_beans < beans_needed) {
        printf("Insufficient coffee beans.\n");
        return 0;
    }
    if (water < water_needed) {
        printf("Insufficient water.\n");
        return 0;
    }
    if (milk < milk_needed) {
        printf("Insufficient milk.\n");
        return 0;
    }
    if (chocolate_syrup < syrup_needed) {
        printf("Insufficient chocolate syrup.\n");
        return 0;
    }
    return 1;
}

// Function to display the menu for purchasing items

void displayPurchaseMenu() {
    printf("\nItems available for purchase:\n");

    // Check for each coffee type and display with availability status
    printf("1. %s (%.2f) ", cappuccino, coffee_price_A);
    if (checkIngredients(8, 30, 70, 0)) {
        printf("\n");
    } else {
        printf(" - Unavailable due to temporarily insufficient ingredients\n");
    }

    printf("2. %s (%.2f) ", mocha, coffee_price_B);
    if (checkIngredients(8, 39, 160, 30)) {
        printf("\n");
    } else {
        printf(" - Unavailable due to temporarily insufficient ingredients\n");
    }

    printf("3. %s (%.2f) ", espresso, coffee_price_C);
    if (checkIngredients(8, 30, 0, 0)) {
        printf("\n");
    } else {
        printf(" - Unavailable due to temporarily insufficient ingredients\n");
    }
    
    printf("0. Cancel\n");
}


// Function to purchase an item
void purchaseItem() {
    while (1) {
        displayPurchaseMenu();

        int choice;
        printf("Enter your choice: ");
        scanf("%d", &choice);

        float selected_price = 0.0f;
        char selected_name[20];
        int beans_needed = 0, water_needed = 0, milk_needed = 0, syrup_needed = 0;

        // Set ingredient requirements based on the choice
        switch (choice) {
            case 1:
                selected_price = coffee_price_A;
                strcpy(selected_name, cappuccino);
                beans_needed = 8;
                water_needed = 30;
                milk_needed = 70;
                syrup_needed = 0;
                break;
            case 2:
                selected_price = coffee_price_B;
                strcpy(selected_name, mocha);
                beans_needed = 8;
                water_needed = 39;
                milk_needed = 160;
                syrup_needed = 30;
                break;
            case 3:
                selected_price = coffee_price_C;
                strcpy(selected_name, espresso);
                beans_needed = 8;
                water_needed = 30;
                milk_needed = 0;
                syrup_needed = 0;
                break;
            case 0:
                printf("Purchase cancelled.\n");
                return;
            default:
                printf("Invalid choice. Purchase cancelled.\n");
                return;
        }

        // Check if ingredients are sufficient
        if (!checkIngredients(beans_needed, water_needed, milk_needed, syrup_needed)) {
            printf("%s is unavailable due to temporarily insufficient ingredients.\n", selected_name);
            return;
        }

        // Confirmation prompt
        int confirm;
        printf("You selected: %s (%.2f). Confirm purchase? (1 for Yes, 0 for No): ", selected_name, selected_price);
        scanf("%d", &confirm);
        if (confirm != 1) {
            printf("Purchase not confirmed. Returning to menu.\n");
            continue; // Restart purchase loop if not confirmed
        }

        // Payment process
        float amount_inserted = 0.0f;
        float coin;
        while (amount_inserted < selected_price) {
            printf("Insert coin: ");
            scanf("%f", &coin);
            if (coin == 1.0f || coin == 0.5f)
                amount_inserted += coin;
            else
                printf("Invalid coin. Please insert a valid coin.\n");
        }

        // Update ingredients and print purchase details
        updateIngredients(beans_needed, water_needed, milk_needed, syrup_needed);
        total_amount += selected_price;
        printf("You have purchased %s for %.2f. Thank you!\n", selected_name, selected_price);
        printf("Change to return: %.2f\n", amount_inserted - selected_price);

        // Check ingredient levels after purchase
        checkLowIngredients();

        break;
    }
}

// Function to check if ingredients are sufficient
int checkIngredients(int beans, int water, int milk, int syrup) {
    return (coffee_beans >= beans && water >= water && milk >= milk && chocolate_syrup >= syrup);
}

// Function to update ingredients after a purchase
void updateIngredients(int beans, int water, int milk, int syrup) {
    coffee_beans -= beans;
    water -= water;
    milk -= milk;
    chocolate_syrup -= syrup;
}

// Function to alert when ingredients are low
void checkLowIngredients() {
    if (coffee_beans <= 10)
        printf("ALERT: Coffee beans are low.\n");
    if (water <= 50)
        printf("ALERT: Water is low.\n");
    if (milk <= 50)
        printf("ALERT: Milk is low.\n");
    if (chocolate_syrup <= 10)

void changeItemPrices() {

    printf("Current prices:\n");
    printf("A - CAPPUCCINO - %.2f\n", coffee_price_A);
    printf("B - MOCHA - %.2f\n", coffee_price_B);
    printf("C - ESPRESSO - %.2f\n", coffee_price_C);

    printf("Enter new price for Cappuccino: ");
    scanf("%f", &coffee_newprice_A);
    coffee_price_A = coffee_newprice_A;

    printf("Enter new price for Mocha: ");
    scanf("%f", &coffee_newprice_B);
    coffee_price_B = coffee_newprice_B;
    
    printf("Enter new price for Espresso: ");
    scanf("%f", &coffee_newprice_C);
    coffee_price_C = coffee_newprice_C;
    printf("Prices updated\n");
}

// Function to restock ingredients
void restockIngredients() {
    printf("Enter amount of coffee beans to add: ");
    int beans;
    scanf("%d", &beans);
    coffee_beans += beans;

    printf("Enter amount of water to add: ");
    int added_water;
    scanf("%d", &added_water);
    water += added_water;

    printf("Enter amount of milk to add: ");
    int added_milk;
    scanf("%d", &added_milk);
    milk += added_milk;

    printf("Enter amount of chocolate syrup to add: ");
    int syrup;
    scanf("%d", &syrup);
    chocolate_syrup += syrup;

    printf("Ingredients restocked successfully.\n");
}

// Function for admin mode
void adminMode() {
    int entered_password;
    printf("Enter admin password: ");
    scanf("%d", &entered_password);

    if (entered_password != admin_password) {
        printf("Incorrect password. Exiting admin mode.\n");
        return;
    }

    while (1) {
        printf("\nAdmin Menu:\n");
        printf("1. Restock Items\n");
        printf("2. Change Item Prices\n");
        printf("3. Display Total Sale\n");
        printf("4. Display Item Availability\n");
        printf("5. Restock Ingredients\n");
        printf("0. Exit Admin Mode\n");

        int admin_choice;
        printf("Enter your choice: ");
        scanf("%d", &admin_choice);

        switch (admin_choice) {
        case 1:
            // Restocking items
            quantity_A = rand() % 16 + 5;
            quantity_B = rand() % 16 + 5;
            quantity_C = rand() % 16 + 5;
            printf("Items replenished.\n");
            break;
        case 2:
            // Change item prices
            changeItemPrices();
            break;
        case 3:
            // Display total sale
            printf("Total Sale Amount: %.2f\n", total_amount);
            printf("Would you like to reset the total sale amount to zero? (1 for Yes, 0 for No): ");
            int reset_choice;
            scanf("%d", &reset_choice);
            if (reset_choice == 1) {
                total_amount = 0.0f;
                printf("Total sale amount reset to zero. Don't forget to collect the money.\n");
            }
            break;
        case 4:
            // Item availability
            printf("Item Availability:\n");
            printf("%s: %d\n", cappuccino, quantity_A);
            printf("%s: %d\n", mocha, quantity_B);
            printf("%s: %d\n", espresso, quantity_C);
            break;
        case 5:
            // Restock ingredients
            restockIngredients();
            break;
        case 0:
            // Exit admin mode
            return;
        default:
            printf("Invalid choice. Try again.\n");
            break;
        }
    }
}

int main() {
    srand(time(NULL));

    while (1) {
        printf("\nMenu:\n");
        printf("1. Purchase Item\n");
        printf("2. Admin Mode\n");
        printf("3. Exit\n");

        int user_choice;
        printf("Enter your choice: ");
        scanf("%d", &user_choice);

        switch (user_choice) {
        case 1:
            purchaseItem();
            break;
        case 2:
            adminMode();
            break;
        case 3:
            printf("Exiting. Thank you!\n");
            return 0;
        default:
            printf("Invalid choice. Try again.\n");
            break;
        }
    }

    return 0;
}
