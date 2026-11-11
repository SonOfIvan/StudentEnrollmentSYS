import sqlite3


# -----------------------
# Database Connection
# -----------------------
class Database:
    def __init__(self):
        self.conn = sqlite3.connect("student_enrollment.db")
        self.cursor = self.conn.cursor()
        self.create_tables()

    def create_tables(self):

        self.cursor.execute("""
        CREATE TABLE IF NOT EXISTS Students (
            student_id INTEGER PRIMARY KEY,
            first_name TEXT NOT NULL,
            last_name TEXT NOT NULL,
            email TEXT UNIQUE NOT NULL,
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
        """)

        self.cursor.execute("""
        CREATE TABLE IF NOT EXISTS Courses (
            course_id INTEGER PRIMARY KEY,
            course_code TEXT UNIQUE NOT NULL,
            course_name TEXT NOT NULL,
            credits INTEGER NOT NULL
        )
        """)

        self.cursor.execute("""
        CREATE TABLE IF NOT EXISTS Enrollment (
            student_id INTEGER,
            course_id INTEGER,
            PRIMARY KEY(student_id, course_id),
            FOREIGN KEY(student_id) REFERENCES Students(student_id),
            FOREIGN KEY(course_id) REFERENCES Courses(course_id)
        )
        """)

        self.conn.commit()


# -----------------------
# Student Class
# -----------------------
class Student:

    def __init__(self, db):
        self.db = db

    def add_student(self, sid, first, last, email):
        try:
            self.db.cursor.execute("""
                INSERT INTO Students
                VALUES (?, ?, ?, ?, CURRENT_TIMESTAMP)
            """, (sid, first, last, email))

            self.db.conn.commit()
            print("Student added successfully!")

        except sqlite3.IntegrityError:
            print("Error: Student ID or Email already exists.")

    def show_students(self):

        self.db.cursor.execute("SELECT * FROM Students")
        students = self.db.cursor.fetchall()

        print("\n--- STUDENTS ---")

        for s in students:
            print(s)


# -----------------------
# Course Class
# -----------------------
class Course:

    def __init__(self, db):
        self.db = db

    def add_course(self, cid, code, name, credits):

        try:
            self.db.cursor.execute("""
                INSERT INTO Courses
                VALUES (?, ?, ?, ?)
            """, (cid, code, name, credits))

            self.db.conn.commit()
            print("Course added successfully!")

        except sqlite3.IntegrityError:
            print("Error: Course ID or Code already exists.")

    def show_courses(self):

        self.db.cursor.execute("SELECT * FROM Courses")
        courses = self.db.cursor.fetchall()

        print("\n--- COURSES ---")

        for c in courses:
            print(c)


# -----------------------
# Enrollment Class
# -----------------------
class Enrollment:

    def __init__(self, db):
        self.db = db

    def enroll_student(self, student_id, course_id):

        try:
            self.db.cursor.execute("""
                INSERT INTO Enrollment
                VALUES (?, ?)
            """, (student_id, course_id))

            self.db.conn.commit()

            print("Enrollment successful!")

        except sqlite3.IntegrityError:
            print("Student is already enrolled.")

    def show_enrollments(self):

        self.db.cursor.execute("""
        SELECT Students.student_id,
               Students.first_name,
               Students.last_name,
               Courses.course_code,
               Courses.course_name
        FROM Enrollment
        JOIN Students
            ON Enrollment.student_id = Students.student_id
        JOIN Courses
            ON Enrollment.course_id = Courses.course_id
        """)

        records = self.db.cursor.fetchall()

        print("\n--- ENROLLMENTS ---")

        for record in records:
            print(record)


# -----------------------
# User Interface
# -----------------------
def main():

    db = Database()

    student = Student(db)
    course = Course(db)
    enrollment = Enrollment(db)

    while True:

        print("""
======== STUDENT ENROLLMENT SYSTEM ========

1. Add Student
2. Add Course
3. Enroll Student
4. Show All Students
5. Show All Courses
6. Show All Enrollments
7. Exit

==========================================
""")

        choice = input("Enter choice: ")

        if choice == "1":

            sid = int(input("Student ID: "))
            first = input("First Name: ")
            last = input("Last Name: ")
            email = input("Email: ")

            student.add_student(sid, first, last, email)

        elif choice == "2":

            cid = int(input("Course ID: "))
            code = input("Course Code: ")
            name = input("Course Name: ")
            credits = int(input("Credits: "))

            course.add_course(cid, code, name, credits)

        elif choice == "3":

            sid = int(input("Student ID: "))
            cid = int(input("Course ID: "))

            enrollment.enroll_student(sid, cid)

        elif choice == "4":
            student.show_students()

        elif choice == "5":
            course.show_courses()

        elif choice == "6":
            enrollment.show_enrollments()

        elif choice == "7":
            print("Goodbye!")
            break

        else:
            print("Invalid choice!")


if __name__ == "__main__":
    main()
