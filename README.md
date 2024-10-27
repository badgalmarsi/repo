#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Constants defining the ingredient requirements for each coffee type
#define ESPRESSO_BEANS 8       // grams of beans for one cup of Espresso
#define ESPRESSO_WATER 30      // milliliters of water for one cup of Espresso

#define CAPPUCCINO_BEANS 8     // grams of beans for one cup of Cappuccino
#define CAPPUCCINO_WATER 30    // milliliters of water for one cup of Cappuccino
#define CAPPUCCINO_MILK 70     // milliliters of milk for one cup of Cappuccino

#define MOCHA_BEANS 8          // grams of beans for one cup of Mocha
#define MOCHA_WATER 30         // milliliters of water for one cup of Mocha
#define MOCHA_MILK 160         // milliliters of milk for one cup of Mocha
#define MOCHA_SYRUP 30         // milliliters of chocolate syrup for one cup of Mocha

// Constants for low ingredient alert threshold and admin password
#define LOW_THRESHOLD 10       // Alert threshold when ingredients are low
#define ADMIN_PASSWORD 1234    // Password to enter admin mode

// Global variables for coffee prices, total sales, and available ingredients in the machine
float espressoPrice = 3.5;
float cappuccinoPrice = 4.5;
float mochaPrice = 5.5;
float total_sales = 0;         // Track total sales of the machine
int beans = 100, water = 1000, milk = 500, syrup = 200; // Initial quantities of ingredients

// Function Prototypes
void displayMenu();           // Displays main menu for the coffee machine
void orderCoffee();           // Allows user to order a coffee type
void adminMode();             // Access admin mode for maintenance and settings
void replenishIngredients();   // Replenishes ingredients to random quantities
void changePrice();           // Changes the prices of coffee types

int main() {
    int choice;  // Variable to store user choice in the main menu

    // Main program loop to display menu and process choices
    while (1) {
        displayMenu();        // Show main menu options
        printf("Select an option (1-3): ");
        scanf("%d", &choice);

        if (choice == 1) {
            orderCoffee();    // Process coffee order
        } else if (choice == 2) {
            adminMode();      // Enter admin mode for maintenance
        } else if (choice == 3) {
            printf("Exiting the application...\n");
            break;            // Exit program loop to end application
        } else {
            printf("Invalid choice. Please try again.\n");
        }
    }
    return 0;                 // End of main function
}

// Function to display the main menu options
void displayMenu() {
    printf("\n--- Coffee Machine Menu ---\n");
    printf("1. Order a coffee\n");
    printf("2. Admin mode\n");
    printf("3. Exit\n");
}

// Function to handle coffee ordering and payment processing
void orderCoffee() {
    int coffeeChoice;        // Variable for user's coffee choice
    float price = 0.0, payment = 0.0;  // Variables for price and total payment by user
    int confirm;             // Variable to confirm user choice

    // Loop to allow user to select coffee type and process order
    while (1) {
        printf("\n--- Available Coffee Types ---\n");

        // Check and display availability of each coffee type based on ingredient quantities
        int espressoAvailable = (beans >= ESPRESSO_BEANS && water >= ESPRESSO_WATER);
        int cappuccinoAvailable = (beans >= CAPPUCCINO_BEANS && water >= CAPPUCCINO_WATER && milk >= CAPPUCCINO_MILK);
        int mochaAvailable = (beans >= MOCHA_BEANS && water >= MOCHA_WATER && milk >= MOCHA_MILK && syrup >= MOCHA_SYRUP);

        if (espressoAvailable) {
            printf("1. Espresso - AED %.2f\n", espressoPrice);
        } else {
            printf("1. Espresso - Unavailable\n");
        }

        if (cappuccinoAvailable) {
            printf("2. Cappuccino - AED %.2f\n", cappuccinoPrice);
        } else {
            printf("2. Cappuccino - Unavailable\n");
        }

        if (mochaAvailable) {
            printf("3. Mocha - AED %.2f\n", mochaPrice);
        } else {
            printf("3. Mocha - Unavailable\n");
        }

        printf("0. Cancel\n");

        // Prompt user to choose a coffee or cancel
        printf("Choose your coffee: ");
        scanf("%d", &coffeeChoice);

        if (coffeeChoice == 0) {   // User chose to cancel
            printf("Order cancelled.\n");
            break;
        }

        // Assign price based on coffee type selected and check availability
        if (coffeeChoice == 1 && espressoAvailable) {
            price = espressoPrice;
        } else if (coffeeChoice == 2 && cappuccinoAvailable) {
            price = cappuccinoPrice;
        } else if (coffeeChoice == 3 && mochaAvailable) {
            price = mochaPrice;
        } else {
            printf("Selected coffee type is unavailable due to insufficient ingredients.\n");
            continue;  // Go back to coffee selection if unavailable
        }

        // Confirm the user’s choice
        printf("Confirm your order (1 for yes, 0 for no): ");
        scanf("%d", &confirm);
        if (confirm == 0) {
            continue;         // Return to coffee selection if not confirmed
        }

        // Payment loop to accept coins until payment covers the price
        while (payment < price) {
            float coin;
            printf("Insert coin (0.5 or 1 AED): ");
            scanf("%f", &coin);
            if (coin == 0.5 || coin == 1) {
                payment += coin;
            } else {
                printf("Invalid coin. Please use 0.5 or 1 AED only.\n");
            }
        }

        // Display payment details and change if any
        printf("You paid: AED %.2f\n", payment);
        if (payment > price) {
            printf("Change returned: AED %.2f\n", payment - price);
        }

        // Deduct ingredients based on coffee type and check thresholds
        if (coffeeChoice == 1) {
            beans -= ESPRESSO_BEANS;
            water -= ESPRESSO_WATER;
        } else if (coffeeChoice == 2) {
            beans -= CAPPUCCINO_BEANS;
            water -= CAPPUCCINO_WATER;
            milk -= CAPPUCCINO_MILK;
        } else if (coffeeChoice == 3) {
            beans -= MOCHA_BEANS;
            water -= MOCHA_WATER;
            milk -= MOCHA_MILK;
            syrup -= MOCHA_SYRUP;
        }

        // Alert if any ingredient falls below threshold
        if (beans <= LOW_THRESHOLD) printf("Alert: Low coffee beans!\n");
        if (water <= LOW_THRESHOLD) printf("Alert: Low water!\n");
        if (milk <= LOW_THRESHOLD) printf("Alert: Low milk!\n");
        if (syrup <= LOW_THRESHOLD) printf("Alert: Low syrup!\n");

        // Update total sales
        total_sales += price;
        printf("Enjoy your coffee!\n");
        break;
    }
}

// Function for admin mode with restricted access using password
void adminMode() {
    int password, adminChoice;

    // Request admin password
    printf("Enter admin password: ");
    scanf("%d", &password);

    if (password != ADMIN_PASSWORD) {  // Verify password
        printf("Incorrect password. Exiting admin mode.\n");
        return;
    }

    // Admin options menu in a loop
    while (1) {
        printf("\n--- Admin Menu ---\n");
        printf("1. Display ingredient quantities\n");
        printf("2. Replenish ingredients\n");
        printf("3. Change coffee prices\n");
        printf("0. Exit\n");
        printf("Select an option: ");
        scanf("%d", &adminChoice);

        // Execute corresponding admin tasks
        if (adminChoice == 1) {
            // Display quantities of each ingredient and total sales
            printf("Beans: %d grams\n", beans);
            printf("Water: %d ml\n", water);
            printf("Milk: %d ml\n", milk);
            printf("Syrup: %d ml\n", syrup);
            printf("Total sales: AED %.2f\n", total_sales);
        } else if (adminChoice == 2) {
            replenishIngredients();  // Replenish all ingredients
        } else if (adminChoice == 3) {
            changePrice();           // Change coffee prices
        } else if (adminChoice == 0) {
            break;                   // Exit admin mode
        } else {
            printf("Invalid choice. Try again.\n");
        }
    }
}

// Function to replenish ingredients randomly within set ranges
void replenishIngredients() {
    srand(time(0));  // Seed for random number generator
    beans = rand() % 100 + 50;
    water = rand() % 500 + 300;
    milk = rand() % 300 + 200;
    syrup = rand() % 200 + 100;
    printf("Ingredients replenished.\n");
}

// Function to allow admin to change prices of coffee types
void changePrice() {
    int coffeeType;
    float newPrice;

    printf("\n--- Change Coffee Prices ---\n");
    printf("1. Change Espresso price (current: AED %.2f)\n", espressoPrice);
    printf("2. Change Cappuccino price (current: AED %.2f)\n", cappuccinoPrice);
    printf("3. Change Mocha price (current: AED %.2f)\n", mochaPrice);
    printf("Choose a coffee type to change its price: ");
    scanf("%d", &coffeeType);

    // Update price based on coffee type selected
    switch (coffeeType) {
        case 1:
            printf("Enter new price for Espresso: ");
            scanf("%f", &newPrice);
            espressoPrice = newPrice;
            break;
        case 2:
            printf("Enter new price for Cappuccino: ");
            scanf("%f", &newPrice);
            cappuccinoPrice = newPrice;
            break;
        case 3:
            printf("Enter new price for Mocha: ");
            scanf("%f", &newPrice);
            mochaPrice = newPrice;
            break;
        default:
            printf("Invalid choice.\n");
            return;
    }
    printf("Price updated successfully.\n");
}
