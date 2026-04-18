# TASK-MANAGER-CPP

#include <iostream>
#include <vector>
#include <fstream>
using namespace std;

class Task {
public:
    string title;
    string category;
    bool isCompleted;

    Task(string t, string c, bool status = false) {
        title = t;
        category = c;
        isCompleted = status;
    }
};

class TaskManager {
private:
    vector<Task> tasks;

public:
    void addTask() {
        string title, category;
        cin.ignore();

        cout << "Enter task title: ";
        getline(cin, title);

        cout << "Enter category: ";
        getline(cin, category);

        tasks.push_back(Task(title, category));
        cout << "Task added successfully!\n";
    }

    void viewTasks(bool showCompleted) {
        if (tasks.empty()) {
            cout << "No tasks available.\n";
            return;
        }

        for (int i = 0; i < tasks.size(); i++) {
            if (tasks[i].isCompleted == showCompleted) {
                cout << i + 1 << ". "
                     << tasks[i].title
                     << " [" << tasks[i].category << "] "
                     << (tasks[i].isCompleted ? "(Completed)" : "(Pending)")
                     << endl;
            }
        }
    }

    void markCompleted() {
        int index;
        cout << "Enter task number to mark completed: ";
        cin >> index;

        if (index > 0 && index <= tasks.size()) {
            tasks[index - 1].isCompleted = true;
            cout << "Task marked as completed!\n";
        } else {
            cout << "Invalid task number.\n";
        }
    }

    void saveToFile() {
        ofstream file("tasks.txt");

        for (auto &task : tasks) {
            file << task.title << "|"
                 << task.category << "|"
                 << task.isCompleted << endl;
        }

        file.close();
        cout << "Tasks saved to file.\n";
    }

    void loadFromFile() {
        ifstream file("tasks.txt");

        if (!file) return;

        tasks.clear();

        string title, category;
        bool status;

        while (getline(file, title, '|') &&
               getline(file, category, '|') &&
               file >> status) {

            file.ignore();
            tasks.push_back(Task(title, category, status));
        }

        file.close();
    }
};

int main() {
    TaskManager manager;
    manager.loadFromFile();

    int choice;

    do {
        cout << "\n===== TASK MANAGER =====\n";
        cout << "1. Add Task\n";
        cout << "2. View Pending Tasks\n";
        cout << "3. View Completed Tasks\n";
        cout << "4. Mark Task as Completed\n";
        cout << "5. Save & Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                manager.addTask();
                break;
            case 2:
                manager.viewTasks(false);
                break;
            case 3:
                manager.viewTasks(true);
                break;
            case 4:
                manager.markCompleted();
                break;
            case 5:
                manager.saveToFile();
                cout << "Exiting...\n";
                break;
            default:
                cout << "Invalid choice.\n";
        }

    } while (choice != 5);

    return 0;
}
