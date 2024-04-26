# Designing a University Course Registration System

In this article, we explore the object-oriented design and implementation of a University Course Registration System using Python3. 

The system facilitates course registration and management for students and universities.

## System Requirements

The University Course Registration System should:

1. **Student Management**: Handle student profiles and academic records.
2. **Course Management**: Manage course details, schedules, and capacities.
3. **Registration Process**: Enable students to register for courses.
4. **Prerequisite Checking**: Ensure students meet course prerequisites.
5. **Enrollment Verification**: Confirm student enrollments in courses.

## Core Use Cases

1. **Registering and Managing Student Profiles**
2. **Adding and Updating Courses**
3. **Enrolling in Courses**
4. **Checking Prerequisites**
5. **Verifying Course Enrollment**

## UML/Class Diagrams

Key Classes:

- `CourseRegistrationSystem`: Manages the system.
- `Student`: Represents a student.
- `Course`: Represents a university course.
- `Enrollment`: Manages student enrollments.

## Python3 Implementation

### Student Class

Manages student information and enrollment records.

```python
from typing import Set

class Student:
    def __init__(self, student_id: str, name: str):
        self.student_id: str = student_id
        self.name: str = name
        self.completed_courses: Set[str] = set()
        self.enrollments: Set['Enrollment'] = set()

    def enroll_in_course(self, course: 'Course') -> bool:
        if course.check_prerequisites(self.completed_courses):
            new_enrollment = Enrollment(self, course)
            self.enrollments.add(new_enrollment)
            course.add_student(self)
            return True
        return False

    def add_completed_course(self, course_id: str):
        self.completed_courses.add(course_id)

```
### Course Class
Represents a university course.
```python
from typing import Set

class Course:
    def __init__(self, course_id: str, title: str, capacity: int):
        self.course_id: str = course_id
        self.title: str = title
        self.capacity: int = capacity
        self.prerequisites: Set[str] = set()
        self.students_enrolled: Set[Student] = set()

    def add_student(self, student: Student) -> bool:
        if len(self.students_enrolled) < self.capacity:
            self.students_enrolled.add(student)
            return True
        return False

    def add_prerequisite(self, prerequisite_course_id: str):
        self.prerequisites.add(prerequisite_course_id)

    def check_prerequisites(self, completed_courses: Set[str]) -> bool:
        return self.prerequisites.issubset(completed_courses)

```
### Enrollment Class
Manages a student's enrollment in a course.
```python
class Enrollment:
    def __init__(self, student: Student, course: Course):
        self.student: Student = student
        self.course: Course = course

```
### CourseRegistrationSystem Class
Manages the course registration system operations.
```python
from typing import List, Optional

class CourseRegistrationSystem:
    def __init__(self):
        self.students: List[Student] = []
        self.courses: List[Course] = []

    def add_student(self, student: Student):
        self.students.append(student)

    def add_course(self, course: Course):
        self.courses.append(course)

    def register_student_for_course(self, student_id: str, course_id: str) -> bool:
        student: Optional[Student] = self.find_student_by_id(student_id)
        course: Optional[Course] = self.find_course_by_id(course_id)

        if student and course:
            return student.enroll_in_course(course)
        return False

    def find_student_by_id(self, student_id: str) -> Optional[Student]:
        for student in self.students:
            if student.student_id == student_id:
                return student
        return None

    def find_course_by_id(self, course_id: str) -> Optional[Course]:
        for course in self.courses:
            if course.course_id == course_id:
                return course
        return None

```

### Example Usage
``` python

def main():
    system = CourseRegistrationSystem()
    system.add_course(Course("CS101", "Intro to Computer Science", 30))
    system.add_course(Course("CS102", "Data Structures", 30))

    student = Student("S001", "John Doe")
    student.add_completed_course("CS101")
    system.add_student(student)

    # Attempt to register the student for a course
    if system.register_student_for_course("S001", "CS102"):
        print(f"Student {student.name} successfully enrolled in CS102.")
    else:
        print("Enrollment failed. Prerequisites not met or course is full.")

if __name__ == "__main__":
    main()

```