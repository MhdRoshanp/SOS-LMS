# 🎓 SOS - School of Skills · Learning Management System

A web-based Learning Management System (LMS) for **SOS - School of Skills**, built with **Python** and **Streamlit** using object-oriented programming. It covers the day-to-day running of a training institute: courses and syllabi, faculty, student enrollment, attendance, grades, fee collection, PDF certificates and live analytics — all in a clean white-and-red interface that matches the SOS logo.

![Python](https://img.shields.io/badge/Python-3.9%2B-A31F24?logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-app-A31F24?logo=streamlit&logoColor=white)
![OOP](https://img.shields.io/badge/Design-Object--Oriented-7E1519)

<!-- Add screenshots here, e.g. put images in a docs/ folder and link them:
![Home](docs/home.png)
![Analytics](docs/analytics.png)
-->

---

## ✨ Features

### 📘 Courses
- Course catalogue shown as cards with fee, duration, faculty and a live seat-fill bar
- Search by title or category
- Add new courses (duplicate IDs are rejected)
- **Course Content** builder: modules → lessons (video, reading, assignment, quiz) with durations

### 🧑‍🏫 Faculty
- Faculty profiles with qualification, experience and assigned courses
- One faculty member per course (assignment page shows all current assignments)
- **Daily work attendance** for faculty, with overall percentage and a month-by-month view

### 🎓 Students
- Student profiles showing enrolled courses, attendance, grades and fee status
- Enrollment with a **maximum of 3 courses per student** and seat-limit checks
- **Day-wise attendance** taken by the faculty who teaches the course, with monthly review
- Grade recording (A+, A, B+, B, C, Fail)
- Per-student progress report

### 💰 Fee Management
- Fees tracked **per course** with **installment payments**
- Overpayment is blocked; every payment gets a receipt number (`RCP-2026-0001`)
- **Downloadable PDF receipts**
- Fee status (Paid / Partial / Unpaid), collection rate, and a pending-fees list exportable to CSV
- Full payment history

### 📜 Certificates
- Issued only when attendance is **≥ 75%** and a grade has been recorded
- **Downloadable PDF certificate** with the SOS logo and a QR code
- Unique certificate numbers (`SOS-2026-C001-0001`), one per student per course
- **Verify Certificate** page to confirm that a certificate is genuine

### 📈 Analytics Dashboard
- KPI cards: enrollments, seat utilisation, fees collected/pending, average attendance
- Charts: seat utilisation, fees collected vs pending, grade distribution, monthly attendance trend (students vs faculty), faculty attendance, monthly fee collection, payments by mode
- At-risk students list (attendance below 75%)

### 🧪 Demo mode
- One click on **Load Demo Data** in the sidebar fills the system with sample students, attendance, grades, installments, syllabi and certificates — ideal for presentations.

---

## 🛠️ Tech Stack

| Purpose | Library |
|---|---|
| Web app / UI | [Streamlit](https://streamlit.io) |
| Charts | [Plotly](https://plotly.com/python/) |
| Tables & data | [pandas](https://pandas.pydata.org) |
| PDF generation (certificates, receipts, QR codes) | [ReportLab](https://www.reportlab.com) |
| Image handling (logo) | [Pillow](https://python-pillow.org) |

---

## 📁 Project Structure

```
.
├── app.py              # Streamlit pages and navigation
├── sos.py              # Core classes: Institution, Course, Faculty, Student,
│                       #   Payment, Certificate, LMSActivities, LMSAnalytics
├── theme.py            # Design system: CSS + HTML card components
├── pdf_utils.py        # Certificate and receipt PDF generation
├── demo_data.py        # Sample data loader for demos
├── sos-logo.png        # SOS logo
├── requirements.txt    # Python dependencies
└── .streamlit/
    └── config.toml     # Base Streamlit theme (white + SOS red)
```

---

## 🚀 Getting Started

### Prerequisites
- Python **3.9 or newer**
- `pip`

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# 2. (Recommended) create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
```

### Run the app

```bash
streamlit run app.py
```

Streamlit opens the app at `http://localhost:8501`. Click **🧪 Load Demo Data** in the sidebar to see every page with sample records.

> **Tip:** run `python sos.py` to try the backend classes in the terminal without the web interface.

---

## 🧭 Quick Walkthrough

1. **Add a course** (or use the three starter courses) and build its syllabus under **Course Content**.
2. **Add faculty** and **assign** them to courses.
3. **Add a student**, then **enroll** them — optionally collecting part of the fee straight away.
4. Faculty take attendance under **Mark Attendance**; record final marks under **Record Grade**.
5. Collect further installments under **Fee Management** and download PDF receipts.
6. Once attendance is ≥ 75% and a grade exists, **issue the certificate** and download the PDF.
7. Anyone can confirm a certificate on the **Verify Certificate** page using its number.
8. Watch everything come together on the **Analytics Dashboard**.

---

## 📏 Business Rules

| Rule | Where it is enforced |
|---|---|
| A student can be enrolled in at most **3 courses** at a time | `Student.can_enroll()` |
| A student cannot enroll in the same course twice | `LMSActivities.enroll_student()` |
| Enrollment requires an available seat | `LMSActivities.enroll_student()` |
| A course can have only one faculty member | `LMSActivities.assign_faculty()` |
| Fee payments cannot exceed the pending balance | `LMSActivities.pay_fee()` |
| Certificate needs **≥ 75% attendance** and a recorded grade | `LMSActivities.issue_certificate()` |
| Each student/course pair gets a single certificate | `LMSActivities.issue_certificate()` |

---

## 🧱 Object-Oriented Design

The project is organised around a small set of classes in `sos.py`:

- **`Institution`** *(abstract base class)* — shared institution details as class variables; `Course`, `Faculty` and `Student` inherit from it.
- **`Course`** — class-level registry of all courses, class methods for searching and listing available courses, and instance methods for the syllabus (modules and lessons).
- **`Faculty`** — private attributes (`__attendance`, `__assigned_courses`, `__salary`) exposed through methods (**encapsulation**).
- **`Student`** — private records of enrollments, per-course fees, payments, attendance and grades.
- **`Payment`** and **`Certificate`** — value objects created by the system.
- **`LMSActivities`** — the operations layer: enrollment, attendance, fees, grades and certificates.
- **`LMSAnalytics`** — read-only reporting that feeds the dashboard and fee reports.

Concepts demonstrated: inheritance, abstraction, encapsulation, class vs instance methods, class variables, and separation of concerns (data model · operations · reporting · UI · design system).

---

## ⚠️ Known Limitations

- **Data is stored in memory only.** Restarting the app resets everything. (Use *Load Demo Data* to refill it quickly.)
- There is no login or role-based access yet — every page is available to everyone.
- PDFs use standard fonts, so amounts are printed as `Rs.` and names in non-Latin scripts will not render.
- The web fonts (Inter, Poppins) load from Google Fonts; offline, the app falls back to system fonts.

## 🔮 Ideas for Future Work

- Persistent storage (SQLite / PostgreSQL)
- Login with Admin, Faculty and Student roles
- Assignments and auto-graded quizzes
- Email / WhatsApp reminders for pending fees and low attendance
- Timetable and notice board
- Student and faculty feedback ratings

---

## 📄 License

Add a license before publishing (for example [MIT](https://choosealicense.com/licenses/mit/)) and describe it here.

## 👤 Author

**Your Name** — built for the LMS competition, School of Skills.
