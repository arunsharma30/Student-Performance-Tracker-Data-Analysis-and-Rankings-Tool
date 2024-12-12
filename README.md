# Student-Performance-Tracker-Data-Analysis-and-Rankings-Tool
import tkinter as tk
from tkinter import messagebox

def get_student_data():
    def submit_data():
        try:
            num_students = int(entry_num_students.get())
            if num_students <= 0:
                raise ValueError
        except ValueError:
            messagebox.showerror("Invalid Input", "Please enter a valid number of students.")
            return

        input_window.destroy()
        collect_student_data(num_students)

    input_window = tk.Tk()
    input_window.title("Enter Number of Students")

    tk.Label(input_window, text="Enter the Number of Students:").grid(row=0, column=0, padx=10, pady=10)
    entry_num_students = tk.Entry(input_window)
    entry_num_students.grid(row=0, column=1, padx=10, pady=10)
    tk.Button(input_window, text="Submit", command=submit_data).grid(row=1, columnspan=2, pady=10)

    input_window.mainloop()

def collect_student_data(num_students):
    students = []

    def add_student():
        name = entry_name.get()
        try:
            physics = int(entry_physics.get())
            chemistry = int(entry_chemistry.get())
            maths = int(entry_maths.get())
            if not name or physics < 0 or chemistry < 0 or maths < 0:
                raise ValueError
        except ValueError:
            messagebox.showerror("Invalid Input", "Please enter valid student data.")
            return

        total_marks = physics + chemistry + maths
        percentage = (total_marks / 300) * 100
        students.append([name, physics, chemistry, maths, total_marks, percentage])

        entry_name.delete(0, tk.END)
        entry_physics.delete(0, tk.END)
        entry_chemistry.delete(0, tk.END)
        entry_maths.delete(0, tk.END)

        if len(students) == num_students:
            student_window.destroy()
            calculate_average_and_topper(students)

    student_window = tk.Tk()
    student_window.title("Enter Student Data")

    tk.Label(student_window, text="Name").grid(row=0, column=0, padx=10, pady=5)
    tk.Label(student_window, text="Physics").grid(row=0, column=1, padx=10, pady=5)
    tk.Label(student_window, text="Chemistry").grid(row=0, column=2, padx=10, pady=5)
    tk.Label(student_window, text="Maths").grid(row=0, column=3, padx=10, pady=5)

    entry_name = tk.Entry(student_window)
    entry_physics = tk.Entry(student_window)
    entry_chemistry = tk.Entry(student_window)
    entry_maths = tk.Entry(student_window)

    entry_name.grid(row=1, column=0, padx=10, pady=5)
    entry_physics.grid(row=1, column=1, padx=10, pady=5)
    entry_chemistry.grid(row=1, column=2, padx=10, pady=5)
    entry_maths.grid(row=1, column=3, padx=10, pady=5)

    tk.Button(student_window, text="Add Student", command=add_student).grid(row=2, columnspan=4, pady=10)

    student_window.mainloop()

def calculate_average_and_topper(students):
    total_physics = total_chemistry = total_maths = 0
    topper_physics = {"name": "", "marks": 0}
    topper_chemistry = {"name": "", "marks": 0}
    topper_maths = {"name": "", "marks": 0}
    
    for student in students:
        name, physics, chemistry, maths = student[0], student[1], student[2], student[3]
        total_physics += physics
        total_chemistry += chemistry
        total_maths += maths

        if physics > topper_physics["marks"]:
            topper_physics = {"name": name, "marks": physics}
        if chemistry > topper_chemistry["marks"]:
            topper_chemistry = {"name": name, "marks": chemistry}
        if maths > topper_maths["marks"]:
            topper_maths = {"name": name, "marks": maths}
    
    num_students = len(students)
    average_physics = total_physics / num_students
    average_chemistry = total_chemistry / num_students
    average_maths = total_maths / num_students

    display_results(students, average_physics, average_chemistry, average_maths, topper_physics, topper_chemistry, topper_maths)

def display_results(students, avg_physics, avg_chemistry, avg_maths, topper_physics, topper_chemistry, topper_maths):
    results_window = tk.Tk()
    results_window.title("Results")

    tk.Label(results_window, text="Average Marks").grid(row=0, column=0, columnspan=2, pady=10)
    tk.Label(results_window, text=f"Physics: {avg_physics:.2f}").grid(row=1, column=0, columnspan=2)
    tk.Label(results_window, text=f"Chemistry: {avg_chemistry:.2f}").grid(row=2, column=0, columnspan=2)
    tk.Label(results_window, text=f"Maths: {avg_maths:.2f}").grid(row=3, column=0, columnspan=2)

    tk.Label(results_window, text="\nTopper in Each Subject").grid(row=4, column=0, columnspan=2, pady=10)
    tk.Label(results_window, text=f"Physics: {topper_physics['name']} ({topper_physics['marks']})").grid(row=5, column=0, columnspan=2)
    tk.Label(results_window, text=f"Chemistry: {topper_chemistry['name']} ({topper_chemistry['marks']})").grid(row=6, column=0, columnspan=2)
    tk.Label(results_window, text=f"Maths: {topper_maths['name']} ({topper_maths['marks']})").grid(row=7, column=0, columnspan=2)

    tk.Label(results_window, text="\nStudent Rankings").grid(row=8, column=0, columnspan=2, pady=10)

    # Sort students by total marks
    students.sort(key=lambda student: student[4], reverse=True)
    for i, student in enumerate(students):
        tk.Label(results_window, text=f"{i + 1}. {student[0]} - Total: {student[4]}, Percentage: {student[5]:.2f}%").grid(row=9 + i, column=0, columnspan=2)

    results_window.mainloop()

def main():
    get_student_data()

if __name__ == "__main__":
    main()
