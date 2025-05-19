# Experiment 1: Entity-Relationship (ER) Diagram

## 🎯 Objective:
To understand and apply the concepts of ER modeling by creating an ER diagram for a real-world application.

## 📚 Purpose:
The purpose of this workshop is to gain hands-on experience in designing ER diagrams that visually represent the structure of a database including entities, relationships, attributes, and constraints.

---

## 🧪 Choose One Scenario:

### 🔹 Scenario 1: University Database
Design a database to manage students, instructors, programs, courses, and student enrollments. Include prerequisites for courses.

**User Requirements:**
- Academic programs grouped under departments.
- Students have admission number, name, DOB, contact info.
- Instructors with staff number, contact info, etc.
- Courses have number, name, credits.
- Track course enrollments by students and enrollment date.
- Add support for prerequisites (some courses require others).

---

### 🔹 Scenario 2: Hospital Database
Design a database for patient management, appointments, medical records, and billing.

**User Requirements:**
- Patient details including contact and insurance.
- Doctors and their departments, contact info, specialization.
- Appointments with reason, time, patient-doctor link.
- Medical records with treatments, diagnosis, test results.
- Billing and payment details for each appointment.

---

## 📝 Tasks:
1. Identify entities, relationships, and attributes.
2. Draw the ER diagram using any tool (draw.io, dbdiagram.io, hand-drawn and scanned).
3. Include:
   - Cardinality & participation constraints
   - Prerequisites for University OR Billing for Hospital
4. Explain:
   - Why you chose the entities and relationships.
   - How you modeled prerequisites or billing.

# ER Diagram Submission - Student Name - KAMALESH K

## Scenario Chosen:
University / Hospital (choose one)

## ER Diagram:
![image](https://github.com/user-attachments/assets/2467e60f-1a27-4626-93ee-30a859b3a414)


## Entities and Attributes:

LOGIN:

Login_ID

Login_username

user_password

Login_details

FACULTY:

Faculty_ID

Faculty_Name

Faculty_email_ID

STUDENTS:

Student_ID

Stu_Name

Stu_ph_no

Stu_Pass

Stu_parent_number

Student_email

AMOUNT / FEE:

amount_pending

Fee_paid

Fee_details

COURSE:

Course_ID

Course_name

Course_hours

Course_enroll

REGISTRATION:

Reg_ID

Reg_capacity

Reg_Date

Reg_stu_ID

COLLEGES:

College_id

College_type

College_address

## Relationships and Constraints:
- Relationship1 (Cardinality, Participation)
- Relationship2 (Cardinality, Participation)

reg_management (between LOGIN and FACULTY/STUDENTS/AMOUNT)

Manages authentication and access

Cardinality: One-to-many from LOGIN to STUDENTS and FACULTY

Participation: Total for STUDENTS and FACULTY (must log in)

Manage (between STUDENTS and COURSE/REGISTRATION)

Manages course registration

Cardinality: One-to-many from STUDENTS to REGISTRATION

Participation: Partial

administration (between COURSE and COLLEGES)

Links courses to the institution

Cardinality: Many-to-one from COURSE to COLLEGES

Participation: Total for COURSE



## Extension (Prerequisite / Billing):
Billing is modeled using the AMOUNT / FEE entity. It includes attributes such as amount_pending, Fee_paid, and Fee_details. This entity is linked indirectly to STUDENTS via LOGIN and reg_management.

Prerequisites could be extended by adding a recursive relationship within the COURSE entity, such as prerequisite_of, to indicate dependency between courses.



## Design Choices:

Each major participant in a college management system (students, faculty, colleges, etc.) is treated as a separate entity with appropriate attributes.

Relationships such as reg_management, Manage, and administration model the actions (login, registration, and administration respectively) with clear cardinalities and participation constraints.

The modular structure allows easy extension, such as adding billing or prerequisites, and fits well with a relational database model.

## RESULT

Thus the concepts of ER modeling by creating an ER diagram for a real-world application for University database is applied and understood.
