#include <iostream>
#include <fstream>
#include <string>
using namespace std;

class Student {
private:
    int rollNo;
    string name;
    int age;
    string course;

public:
    void input() {
        cout << "\nEnter Roll Number: ";
        cin >> rollNo;
        cin.ignore();

        cout << "Enter Name: ";
        getline(cin, name);

        cout << "Enter Age: ";
        cin >> age;
        cin.ignore();

        cout << "Enter Course: ";
        getline(cin, course);
    }

    void display() {
        cout << "\n-----------------------------";
        cout << "\nRoll Number : " << rollNo;
        cout << "\nName        : " << name;
        cout << "\nAge         : " << age;
        cout << "\nCourse      : " << course;
        cout << "\n-----------------------------\n";
    }

    int getRollNo() {
        return rollNo;
    }

    string getData() {
        return to_string(rollNo) + "|" + name + "|" + to_string(age) + "|" + course;
    }

    void setData(string line) {
        int pos1 = line.find("|");
        int pos2 = line.find("|", pos1 + 1);
        int pos3 = line.find("|", pos2 + 1);

        rollNo = stoi(line.substr(0, pos1));
        name = line.substr(pos1 + 1, pos2 - pos1 - 1);
        age = stoi(line.substr(pos2 + 1, pos3 - pos2 - 1));
        course = line.substr(pos3 + 1);
    }
};


void addStudent() {
    Student s;
    ofstream file("students.txt", ios::app);

    s.input();
    file << s.getData() << endl;

    file.close();

    cout << "\nStudent Added Successfully!\n";
}


void displayStudents() {
    ifstream file("students.txt");
    string line;

    Student s;

    cout << "\n===== STUDENT RECORDS =====\n";

    while (getline(file, line)) {
        s.setData(line);
        s.display();
    }

    file.close();
}

void searchStudent() {
    int roll;
    bool found = false;

    cout << "\nEnter Roll Number to Search: ";
    cin >> roll;

    ifstream file("students.txt");
    string line;
    Student s;

    while (getline(file, line)) {
        s.setData(line);

        if (s.getRollNo() == roll) {
            cout << "\nStudent Found!\n";
            s.display();
            found = true;
            break;
        }
    }

    file.close();

    if (!found) {
        cout << "\nStudent Not Found!\n";
    }
}


void deleteStudent() {
    int roll;
    bool found = false;

    cout << "\nEnter Roll Number to Delete: ";
    cin >> roll;

    ifstream file("students.txt");
    ofstream temp("temp.txt");

    string line;
    Student s;

    while (getline(file, line)) {
        s.setData(line);

        if (s.getRollNo() != roll) {
            temp << line << endl;
        } else {
            found = true;
        }
    }

    file.close();
    temp.close();

    remove("students.txt");
    rename("temp.txt", "students.txt");

    if (found)
        cout << "\nStudent Deleted Successfully!\n";
    else
        cout << "\nStudent Not Found!\n";
}


void updateStudent() {
    int roll;
    bool found = false;

    cout << "\nEnter Roll Number to Update: ";
    cin >> roll;

    ifstream file("students.txt");
    ofstream temp("temp.txt");

    string line;
    Student s;

    while (getline(file, line)) {
        s.setData(line);

        if (s.getRollNo() == roll) {
            cout << "\nEnter New Details:\n";
            s.input();
            temp << s.getData() << endl;
            found = true;
        } else {
            temp << line << endl;
        }
    }

    file.close();
    temp.close();

    remove("students.txt");
    rename("temp.txt", "students.txt");

    if (found)
        cout << "\nStudent Updated Successfully!\n";
    else
        cout << "\nStudent Not Found!\n";
}

int main() {
    int choice;

    do {
        cout << "\n========== STUDENT MANAGEMENT SYSTEM ==========\n";
        cout << "1. Add Student\n";
        cout << "2. Display All Students\n";
        cout << "3. Search Student\n";
        cout << "4. Update Student\n";
        cout << "5. Delete Student\n";
        cout << "6. Exit\n";

        cout << "\nEnter Your Choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            addStudent();
            break;

        case 2:
            displayStudents();
            break;

        case 3:
            searchStudent();
            break;

        case 4:
            updateStudent();
            break;

        case 5:
            deleteStudent();
            break;

        case 6:
            cout << "\nThank You!\n";
            break;

        default:
            cout << "\nInvalid Choice!\n";
        }

    } while (choice != 6);

    return 0;
}
