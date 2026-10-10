# Nursery-Management-System
#include <iostream>
#include <iomanip>
using namespace std;

// PLANT STRUCTURE
struct Plant {
    int id, stock;
    string name;
    float price;
    string type;        // NEW
    string watering;    // NEW
};

// PURCHASE STRUCTURE
struct Purchase {
    int id;
    string name;
    float price;
    int quantity;
    string customerName;
    float totalCost;
};

Plant p[100];
Purchase purchases[100];

int countP = 0;
int purchaseCount = 0;

// ADD PLANT
void addPlant() {
    int n;
    cout << "How many plants do you want to add? ";
    cin >> n;

    for(int i = 0; i < n; i++) {
        if(countP >= 100) {
            cout << "Storage Full!\n";
            return;
        }

        cout << "\n--- Plant " << i+1 << " Details ---\n";

        cout << "Plant ID   : ";
        cin >> p[countP].id;

        // Check duplicate ID
        bool duplicate = false;
        for(int j = 0; j < countP; j++) {
            if(p[j].id == p[countP].id) {
                cout << "ID already exists!\n";
                duplicate = true;
                break;
            }
        }
        if(duplicate) {
            i--;
            continue;
        }

        cout << "Plant Name : ";
        cin.ignore();
        getline(cin, p[countP].name);

        cout << "Plant Type (Flowering/Non-Flowering): ";
        getline(cin, p[countP].type);

        cout << "Watering Schedule : ";
        getline(cin, p[countP].watering);

        cout << "Price      : ";
        cin >> p[countP].price;

        cout << "Stock      : ";
        cin >> p[countP].stock;

        if(p[countP].price < 0 || p[countP].stock < 0) {
            cout << "Invalid input! Try again.\n";
            i--;
            continue;
        }

        countP++;
    }
}

// DISPLAY PLANTS
void display() {
    if(countP == 0) {
        cout << "No plants available\n";
        return;
    }

    cout << fixed << setprecision(2);
    cout << "\n--- Plant List ---\n";

    for(int i = 0; i < countP; i++) {
        cout << "\nPlant " << i+1 << endl;
        cout << "Plant ID   : " << p[i].id << endl;
        cout << "Plant Name : " << p[i].name << endl;
        cout << "Type       : " << p[i].type << endl;
        cout << "Watering   : " << p[i].watering << endl;
        cout << "Price      : " << p[i].price << endl;
        cout << "Stock      : " << p[i].stock << endl;
    }
}

// SEARCH
void searchPlant() {
    int id;
    cout << "Enter Plant ID to search: ";
    cin >> id;

    for(int i = 0; i < countP; i++) {
        if(p[i].id == id) {
            cout << "\n--- Plant Found ---\n";
            cout << "Plant ID   : " << p[i].id << endl;
            cout << "Plant Name : " << p[i].name << endl;
            cout << "Type       : " << p[i].type << endl;
            cout << "Watering   : " << p[i].watering << endl;
            cout << "Price      : " << p[i].price << endl;
            cout << "Stock      : " << p[i].stock << endl;
            return;
        }
    }
    cout << "Plant not found\n";
}

// UPDATE
void updatePlant() {
    int id;
    cout << "Enter Plant ID to update: ";
    cin >> id;

    for(int i = 0; i < countP; i++) {
        if(p[i].id == id) {
            cout << "\n--- Update Plant ---\n";

            cout << "Enter new Name : ";
            cin.ignore();
            getline(cin, p[i].name);

            cout << "Enter new Type : ";
            getline(cin, p[i].type);

            cout << "Enter Watering Schedule: ";
            getline(cin, p[i].watering);

            cout << "Enter new Price: ";
            cin >> p[i].price;

            cout << "Enter new Stock: ";
            cin >> p[i].stock;

            cout << "Plant Updated Successfully!\n";
            return;
        }
    }
    cout << "Plant not found\n";
}

// DELETE
void deletePlant() {
    int id;
    cout << "Enter Plant ID to delete: ";
    cin >> id;

    for(int i = 0; i < countP; i++) {
        if(p[i].id == id) {

            for(int j = i; j < countP - 1; j++) {
                p[j] = p[j + 1];
            }
            countP--;

            cout << "Plant deleted successfully!\n";
            return;
        }
    }
    cout << "Plant not found\n";
}

// BUY PLANT + BILLING
void buyPlant() {
    int id, qty;
    string customer;

    cout << "Enter Customer Name: ";
    cin.ignore();
    getline(cin, customer);

    cout << "Enter Plant ID to buy: ";
    cin >> id;

    cout << "Enter Quantity : ";
    cin >> qty;

    for(int i = 0; i < countP; i++) {
        if(p[i].id == id) {

            if(p[i].stock >= qty) {
                float total = p[i].price * qty;
                p[i].stock -= qty;

                purchases[purchaseCount].id = p[i].id;
                purchases[purchaseCount].name = p[i].name;
                purchases[purchaseCount].price = p[i].price;
                purchases[purchaseCount].quantity = qty;
                purchases[purchaseCount].customerName = customer;
                purchases[purchaseCount].totalCost = total;

                purchaseCount++;

                cout << fixed << setprecision(2);
                cout << "\n--- BILL ---\n";
                cout << "Customer Name : " << customer << endl;
                cout << "Plant Name    : " << p[i].name << endl;
                cout << "Type          : " << p[i].type << endl;
                cout << "Watering      : " << p[i].watering << endl;
                cout << "Price         : " << p[i].price << endl;
                cout << "Quantity      : " << qty << endl;
                cout << "Total Bill    : " << total << endl;

                cout << "Purchase Successful!\n";
            } else {
                cout << "Insufficient Stock\n";
            }
            return;
        }
    }
    cout << "Plant not found\n";
}

// VIEW PURCHASES
void viewPurchases() {
    if(purchaseCount == 0) {
        cout << "No purchases yet\n";
        return;
    }

    float grandTotal = 0;

    cout << fixed << setprecision(2);
    cout << "\n--- Purchase History ---\n";

    for(int i = 0; i < purchaseCount; i++) {
        cout << "\nPurchase " << i+1 << endl;
        cout << "Customer Name : " << purchases[i].customerName << endl;
        cout << "Plant ID      : " << purchases[i].id << endl;
        cout << "Plant Name    : " << purchases[i].name << endl;
        cout << "Price         : " << purchases[i].price << endl;
        cout << "Quantity      : " << purchases[i].quantity << endl;
        cout << "Total Cost    : " << purchases[i].totalCost << endl;

        grandTotal += purchases[i].totalCost;
    }

    cout << "\nTotal Sales Amount: " << grandTotal << endl;
}

// MAIN
int main() {
    int choice;

    do {
        cout << "\n--- Nursery Management Menu ---\n";
        cout << "1. Add Plant\n";
        cout << "2. View Plants\n";
        cout << "3. Search Plant\n";
        cout << "4. Update Plant\n";
        cout << "5. Delete Plant\n";
        cout << "6. Buy Plant\n";
        cout << "7. View Purchases\n";
        cout << "8. Exit\n";

        cout << "Enter your choice: ";
        cin >> choice;

        switch(choice) {
            case 1: addPlant(); break;
            case 2: display(); break;
            case 3: searchPlant(); break;
            case 4: updatePlant(); break;
            case 5: deletePlant(); break;
            case 6: buyPlant(); break;
            case 7: viewPurchases(); break;
            case 8: cout << "Exiting program...\n"; break;
            default: cout << "Invalid choice!\n";
        }

    } while(choice != 8);

    return 0;
}
