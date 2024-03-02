# Coding-Raja-technogies-internship

import json
import os
from datetime import datetime

class TaskManager:
    def __init__(self):
        self.tasks = []
        self.filepath = "tasks.json"
        self.load_tasks()

    def load_tasks(self):
        if os.path.exists(self.filepath):
            with open(self.filepath, "r") as file:
                self.tasks = json.load(file)

    def save_tasks(self):
        with open(self.filepath, "w") as file:
            json.dump(self.tasks, file)

    def add_task(self, title, priority="low", due_date=None):
        task = {"title": title, "priority": priority, "due_date": due_date, "completed": False}
        self.tasks.append(task)
        self.save_tasks()

    def remove_task(self, index):
        del self.tasks[index]
        self.save_tasks()

    def mark_task_completed(self, index):
        self.tasks[index]["completed"] = True
        self.save_tasks()

    def list_tasks(self):
        if not self.tasks:
            print("No tasks found.")
            return

        print("Tasks:")
        for index, task in enumerate(self.tasks):
            status = "Completed" if task["completed"] else "Pending"
            priority = task["priority"].capitalize()
            due_date = task["due_date"] if task["due_date"] else "No due date"
            print(f"{index + 1}. {task['title']} - Priority: {priority}, Due Date: {due_date}, Status: {status}")

def main():
    task_manager = TaskManager()

    while True:
        print("\n1. Add Task")
        print("2. Remove Task")
        print("3. Mark Task as Completed")
        print("4. List Tasks")
        print("5. Exit")
        choice = input("Enter your choice: ")

        if choice == "1":
            title = input("Enter task title: ")
            priority = input("Enter priority (low/medium/high): ").lower()
            due_date = input("Enter due date (YYYY-MM-DD), press Enter for no due date: ")
            if due_date:
                due_date = datetime.strptime(due_date, "%Y-%m-%d").strftime("%Y-%m-%d")
            task_manager.add_task(title, priority, due_date)
            print("Task added successfully.")
        elif choice == "2":
            task_manager.list_tasks()
            index = int(input("Enter task number to remove: ")) - 1
            task_manager.remove_task(index)
            print("Task removed successfully.")
        elif choice == "3":
            task_manager.list_tasks()
            index = int(input("Enter task number to mark as completed: ")) - 1
            task_manager.mark_task_completed(index)
            print("Task marked as completed.")
        elif choice == "4":
            task_manager.list_tasks()
        elif choice == "5":
            print("Exiting...")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
