import streamlit as st

#---------LOGIN--------
if"logged_in" not in st.session_state:
    st.session_state.logged_in=False

if "students" not in st.session_state:
    st.session_state.students = []

def login_page():
    st.title("LOGIN PAGE")

    username=st.text_input("username")
    password=st.text_input("password", type="password")

    if st.button("login"):
        if username=="admin" and password=="1234":
            st.session_state.logged_in= True
            st.success("LOGIN SUCCESFUL")
            st.experimental_rerun()
        else:
            st.error("Invalid username and password")

# -------- STUDENT --------
class Student:
    def __init__(self, roll, name, marks):
        self.roll = roll
        self.name = name
        self.marks = marks


# -------- DISPLAY FUNCTION --------
def display(students):
    st.subheader("Student Records")
    for s in students:
        st.write(f"Roll No: {s.roll} | Name: {s.name} | Marks: {s.marks}")


# -------- MARKS --------
def bubble_sort_by_marks(students):
    n = len(students)
    for i in range(n - 1):
        for j in range(n - i - 1):
            if students[j].marks > students[j + 1].marks:
                students[j], students[j + 1] = students[j + 1], students[j]


# -------- NAME --------
def bubble_sort_by_name(students):
    n = len(students)
    for i in range(n - 1):
        for j in range(n - i - 1):
            if students[j].name > students[j + 1].name:
                students[j], students[j + 1] = students[j + 1], students[j]


# -------- ROLL --------
def bubble_sort_by_roll(students):
    n = len(students)
    for i in range(n - 1):
        for j in range(n - i - 1):
            if students[j].roll > students[j + 1].roll:
                students[j], students[j + 1] = students[j + 1], students[j]


# -------- SEARCH STUDENT --------
def search_student(students, roll_no):
    for s in students:
        if s.roll == roll_no:
            st.success("Student Found")
            st.write(f"Roll No: {s.roll}")
            st.write(f"Name: {s.name}")
            st.write(f"Marks: {s.marks}")
            return
    st.error("Student Not Found")
#--------------------------------------------------

if not st.session_state.logged_in:
    login_page()
    st.stop()

#---------MAIN APP-------------------
st.title("🎓 Student Record Management System")


if "students" not in st.session_state:
    st.session_state.students = []

menu = st.sidebar.selectbox(
    "Menu",
    (
        "Add Student",
        "Display Students",
        "Sort by Marks",
        "Sort by Name",
        "Sort by Roll No",
        "Search Student",
    ),
)

# -------- ADD STUDENT --------
if menu == "Add Student":
    st.subheader("Add Student")

    roll = st.number_input("Enter Roll No", min_value=1, step=1)
    name = st.text_input("Enter Name")
    marks = st.number_input("Enter Marks", min_value=0.0, max_value=100.0)

    if st.button("Add"):
        st.session_state.students.append(Student(roll, name, marks))
        st.success("Student Added Successfully!")

# -------- DISPLAY --------
elif menu == "Display Students":
    if not st.session_state.students:
        st.warning("No students available")
    else:
        display(st.session_state.students)

# -------- MARKS --------
elif menu == "Sort by Marks":
    bubble_sort_by_marks(st.session_state.students)
    st.success("Sorted by Marks")
    display(st.session_state.students)

# -------- NAME --------
elif menu == "Sort by Name":
    bubble_sort_by_name(st.session_state.students)
    st.success("Sorted by Name")
    display(st.session_state.students)

# --------ROLL --------
elif menu == "Sort by Roll No":
    bubble_sort_by_roll(st.session_state.students)
    st.success("Sorted by Roll No")
    display(st.session_state.students)

# -------- SEARCH --------
elif menu == "Search Student":
    roll_no = st.number_input("Enter Roll No to Search", min_value=1, step=1)
    if st.button("Search"):
        search_student(st.session_state.students, roll_no)
