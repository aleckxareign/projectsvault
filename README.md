# projectsvault
#include <iostream>
#include <iomanip>

using namespace std;


struct Calculation {
    double num1;
    double num2;
    char operation;
    double result;
};


void addCalculation(Calculation*& history, int& count, double& lastResult) {
    double num1, num2, result;
    char operation;

    cout << "\nEnter first number: ";
    cin >> num1;
    cout << "Enter operator (+, -, *, /): ";
    cin >> operation;
    cout << "Enter second number: ";
    cin >> num2;

    if (operation == '+') {
        result = num1 + num2;
    } else if (operation == '-') {
        result = num1 - num2;
    } else if (operation == '*') {
        result = num1 * num2;
    } else if (operation == '/') {
        if (num2 != 0) {
            result = num1 / num2;
        } else {
            cout << "Error: Division by zero is not allowed!" << endl;
            return;
        }
    } else {
        cout << "Invalid operator!" << endl;
        return;
    }

    
    Calculation* newHistory = new Calculation[count + 1];
    for (int i = 0; i < count; i++) {
        newHistory[i] = history[i];
    }
    newHistory[count] = {num1, num2, operation, result};

    delete[] history;
    history = newHistory;
    count++;

    lastResult = result; 
    cout << "Calculation added successfully!" << endl;
}


void displayHistory(Calculation* history, int count) {
    if (count == 0) {
        cout << "No calculations in history." << endl;
        return;
    }

    cout << "\nCalculation History:" << endl;
    cout << left << setw(10) << "Index" << setw(15) << "Operation" << setw(10) << "Result" << endl;

    for (int i = 0; i < count; i++) {
        cout << left << setw(10) << i + 1
             << history[i].num1 << " " << history[i].operation << " " << history[i].num2
             << " = " << history[i].result << endl;
    }
    cout << endl;
}


void updateCalculation(Calculation* history, int count, double& lastResult) {
    int index;
    cout << "\nEnter the index of the calculation to update (1 to " << count << "): ";
    cin >> index;

    if (index < 1 || index > count) {
        cout << "Invalid index!" << endl;
        return;
    }

    index--; 
    cout << "\nUpdating calculation at index " << index + 1 << endl;

    double num1, num2, result;
    char operation;

    cout << "Enter first number: ";
    cin >> num1;
    cout << "Enter operator (+, -, *, /): ";
    cin >> operation;
    cout << "Enter second number: ";
    cin >> num2;

    if (operation == '+') {
        result = num1 + num2;
    } else if (operation == '-') {
        result = num1 - num2;
    } else if (operation == '*') {
        result = num1 * num2;
    } else if (operation == '/') {
        if (num2 != 0) {
            result = num1 / num2;
        } else {
            cout << "Error: Division by zero is not allowed!" << endl;
            return;
        }
    } else {
        cout << "Invalid operator!" << endl;
        return;
    }

    history[index] = {num1, num2, operation, result};
    lastResult = result;
    cout << "Calculation updated successfully!" << endl;
}

int main() {
    Calculation* history = nullptr; 
    int count = 0;                  
    double lastResult = 0.0;        
    bool hasLastResult = false;     
    int choice;

    do {
        
        if (hasLastResult) {
            cout << "\nLast Calculation Result: " << setprecision(2) << fixed << lastResult << endl;
        }

       
    cout << "\nWelcome to your calculator!" << endl;
        cout << "Press [1] to Add a Calculation" << endl;
        cout << "Press [2] to View Calculation History" << endl;
        cout << "Press [3] to Update a Calculation" << endl;
        cout << "Press [4] to Clear Calculation History" << endl;
        cout << "Press [5] to Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                addCalculation(history, count, lastResult);
                hasLastResult = true;
                break;
            case 2:
                displayHistory(history, count);
                break;
            case 3:
                updateCalculation(history, count, lastResult);
                break;
            case 4:
                delete[] history;
                history = nullptr;
                count = 0;
                hasLastResult = false;
                cout << "\nCalculation history cleared!" << endl;
                break;
            case 5:
                cout << "Exiting program..." << endl;

                if (count > 0) {
                    cout << "\nCalculation History before exiting:" << endl;
                    displayHistory(history, count);
                } 
                else {
                    cout << "No calculations in history to display." << endl;
                }
                delete[] history;
                history = nullptr;
                break;
            default:
                cout << "Invalid choice. Please try again!" << endl;
                break;
        }
    } while (choice != 5);

    return 0;
}
