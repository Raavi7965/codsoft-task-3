# codsoft-task-3
import json
import os
from datetime import datetime

def load_tasks():
    if os.path.exists("tasks.json"):
        with open("tasks.json", "r") as file:
            tasks = json.load(file)
        return tasks
    else:
        return {"tasks": []}

def save_tasks(tasks):
    with open("tasks.json", "w") as file:
        json.dump(tasks, file)

def show_tasks(tasks):
    if tasks["tasks"]:
        print("Your To-Do List:")
        for i, task in enumerate(tasks["tasks"], start=1):
            print(f"{i}. {task['title']} - {task['date']}")
    else:
        print("Your To-Do List is empty.")

def add_task(tasks):
    title = input("Enter task title: ")
    date_str = input("Enter task due date (YYYY-MM-DD): ")
    
    try:
        date = datetime.strptime(date_str, "%Y-%m-%d").strftime("%Y-%m-%d")
    except ValueError:
        print("Invalid date format. Please use YYYY-MM-DD.")
        return

    tasks["tasks"].append({"title": title, "date": date})
    save_tasks(tasks)
    print("Task added successfully!")

def delete_task(tasks):
    show_tasks(tasks)
    try:
        index = int(input("Enter the task number to delete: ")) - 1
        if 0 <= index < len(tasks["tasks"]):
            del tasks["tasks"][index]
            save_tasks(tasks)
            print("Task deleted successfully!")
        else:
            print("Invalid task number.")
    except ValueError:
        print("Invalid input. Please enter a valid number.")

def main():
    tasks = load_tasks()

    while True:
        print("\n1. Show Tasks\n2. Add Task\n3. Delete Task\n4. Exit")
        choice = input("Enter your choice (1-4): ")

        if choice == "1":
            show_tasks(tasks)
        elif choice == "2":
            add_task(tasks)
        elif choice == "3":
            delete_task(tasks)
        elif choice == "4":
            print("Exiting To-Do List application. Goodbye!")
            break
        else:
            print("Invalid choice. Please enter a number between 1 and 4.")

if __name__ == "__main__":
    main()
