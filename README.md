
import tkinter as tk
from datetime import datetime
from tkinter import messagebox

class TodoListApp:
    def __init__(self, root):
        self.root = root
        self.root.title("TO DO LIST")
        self.tasks = []
        self.completed_tasks = []

       
        self.task_entry = tk.Entry(root, width=50)
        self.task_entry.pack(padx=10)

       
        self.task_button = tk.Button(root, text="Add Task", command=self.add_task)
        self.task_button.pack(padx=12, pady=10)

       
        self.task_frame = tk.Frame(root)
        self.task_frame.pack(padx=12)

        
        self.remove_task_button = tk.Button(root, text="Remove Task", command=self.remove_task)
        self.remove_task_button.pack(padx=12)

        
        self.completed_frame = tk.Frame(root)
        self.completed_frame.pack(pady=13)

        self.completed_label = tk.Label(root, text="Completed Tasks")
        self.completed_label.pack()

        self.completed_listbox = tk.Listbox(root, width=50, height=10)
        self.completed_listbox.pack(padx=21)

        self.update_task_listbox()

    def add_task(self):
        task = self.task_entry.get()
        if task:
            var = tk.BooleanVar()
            self.tasks.append((task, var))
            self.update_task_listbox()
            self.task_entry.delete(0, tk.END)
        else:
            messagebox.showwarning("Warning", "You must enter a task.")

    def remove_task(self):
        for task, var in self.tasks:
            if var.get():
                self.completed_tasks.append((task, datetime.now()))
        self.tasks = [(task, var) for task, var in self.tasks if not var.get()]
        self.update_task_listbox()
        self.update_completed_listbox()

    def update_task_listbox(self):
       
        for widget in self.task_frame.winfo_children():
            widget.destroy()
        for task, var in self.tasks:
            checkbutton = tk.Checkbutton(self.task_frame, text=task, variable=var)
            checkbutton.pack(anchor="w")

    def update_completed_listbox(self):
        self.completed_listbox.delete(0, tk.END)
        for task, timestamp in self.completed_tasks:
            self.completed_listbox.insert(tk.END, f"{task} - completed on {timestamp.strftime('%Y-%m-%d %H:%M:%S')}")

if __name__ == "__main__":
    root = tk.Tk()
    app = TodoListApp(root)
    root.mainloop()
