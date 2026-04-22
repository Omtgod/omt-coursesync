================================================================
  COURSESYNC — PROJECT DOCUMENTATION
  Developer  : Aman Choudhary (GitHub: Omtgod)
  Last Updated: April 2026
================================================================

WEBSITE NAME
------------
  CourseSync https://bcatrackerbyomt.netlify.app/
  (Previously known as: Study Tracker / BCA Tracker)

WHAT IS THIS PROJECT?
----------------------
  CourseSync is a web-based Syllabus Tracker built for students.
  On this platform, students can select their university, course,
  and semester, then track their syllabus topic by topic.
  Each topic can be marked as complete, personal notes can be
  written, YouTube or PDF links can be saved, and overall
  study progress can be monitored through an insights dashboard.

TECHNOLOGIES USED
-----------------
  - HTML5                  (Structure)
  - CSS3 / Vanilla CSS     (Dark glassmorphism design)
  - JavaScript (Vanilla)   (No frameworks used)
  - Firebase Firestore     (Global admin resources — YouTube/PDF links)
  - LocalStorage           (User data, progress, notes — offline-first)
  - Chart.js               (Doughnut progress chart on dashboard)
  - Font Awesome 6         (Icons throughout the UI)
  - Google Fonts           (Outfit + JetBrains Mono)

================================================================
  FEATURES BUILT SO FAR
================================================================

1. USER AUTHENTICATION (Login / Sign Up)
   ----------------------------------------
   - Username and password based local authentication system.
   - Supports both Sign Up and Login flows.
   - User credentials are stored securely in localStorage.
   - Old "BCA Tracker" user data is automatically migrated to
     the new CourseSync storage format on first login.

2. UNIVERSITY / COURSE SELECTION SYSTEM  [NEW — April 2026]
   -----------------------------------------------------------
   - After logging in, the user first selects their University.
   - Then the available Courses within that university are shown.
   - Currently available: Ranchi University > B.Sc. (CA).
   - Additional universities and courses can be added in the
     future without any core code changes — just update data.js.
   - "More Universities / More Courses — Coming Soon" placeholder
     cards are shown in the selector grids.
   - A "Switch Course" button allows the user to change their
     selection at any time from the sidebar.

3. SETUP WIZARD
   ------------
   - Shown the first time a user logs into a new course.
   - The user selects their active semester.
   - Optional subjects (GE, AECC electives) can be chosen.
   - Core course subjects are automatically enrolled.
   - The wizard displays context info — selected University & Course.

4. SYLLABUS DATA STRUCTURE  [RESTRUCTURED — April 2026]
   ------------------------------------------------------
   Navigation Hierarchy:
     University --> Course --> Semester --> Subject --> Topics

   File: data.js
   Data Format:
     universitiesData[
       {
         id, name, shortName, location,
         courses: [
           {
             id, name, fullName, duration,
             semesters: [
               {
                 semester,
                 courses: [ { code, name, topics: [...] } ]
               }
             ]
           }
         ]
       }
     ]

   Ranchi University — B.Sc. (CA) currently contains:
     - Semester I   : C1 (C/C++), C2 (CSA), AECC-1, GE-1 options
     - Semester II  : C3 (Java), C4 (Discrete Structures), AECC-2, GE-2
     - Semester III : C5 (Data Structures), C6 (OS), C7 (Networks), GE-3
     - Semester IV  : C8 (DAA), C9 (Software Engg.), C10 (DBMS), GE-4
     - Semester V   : C11 (Internet Technology), C12 (Theory of Computation)
     - Semester VI  : C13 (Artificial Intelligence), C14 (Computer Graphics)

5. TOPIC PROGRESS TRACKING
   ------------------------
   - Each topic has a checkbox next to it.
   - Checking a topic marks it as "completed" with a strikethrough.
   - A progress counter (e.g. 4/11) is shown in each subject card.
   - Data is saved to localStorage — persists after page close.
   - Previously completed topics are remembered after refresh.

6. PERSONAL NOTES SYSTEM
   ----------------------
   - A Notes modal opens for every individual topic.
   - Users can save a personal YouTube or PDF link for that topic.
   - A personal text note can also be written.
   - If a note or link has been saved, the note button turns gold
     as a visual indicator.
   - Clicking the link icon opens the saved resource directly.

7. ADMIN GLOBAL RESOURCES (Firebase Integration)
   -----------------------------------------------
   - A separate Admin Panel allows the admin to set YouTube links
     and PDF links globally for any topic in the syllabus.
   - These links are stored in Firebase Firestore.
   - When a student opens the app, global resources are loaded
     automatically.
   - The student's Notes modal shows an "Official Reference"
     section with the admin-provided link for that topic.

8. INSIGHTS DASHBOARD
   -------------------
   - Overall syllabus completion percentage displayed as a
     Doughnut chart.
   - Exam countdown: user can set their exam date and see
     how many days are left.
   - Total lifetime focus time tracked (accumulated from
     Pomodoro sessions).
   - Total notes count displayed.
   - Activity Heatmap — a GitLab-style grid showing daily
     study activity for the last 91 days.
   - "Up Next" section — shows the top 3 incomplete topics
     across all selected subjects with quick jump links.

9. POMODORO FOCUS TIMER
   ----------------------
   - A built-in study session timer (default 25 minutes,
     fully customizable by the user).
   - Start / Pause / Reset controls.
   - A mini countdown is shown in the header bar during a session.
   - On completion: a sound alert plays and focus time is recorded.
   - Completed focus time is accumulated and shown on the dashboard.

10. STUDY STREAK SYSTEM
    --------------------
    - A "Day Streak" counter is displayed in the sidebar.
    - The streak increments for every day the user studies.
    - Missing a day resets the streak back to zero.

11. THEME TOGGLE (Dark / Light Mode)
    -----------------------------------
    - Supports both Dark mode (default) and Light mode.
    - Monochrome silver/charcoal design in both themes.
    - Theme preference is saved to localStorage.

12. UI / DESIGN FEATURES
    ----------------------
    - Dark glassmorphism aesthetic with frosted panels.
    - Sharp corners with neon silver accent highlights.
    - Web Audio API powered UI sound effects:
        hover, click, task-check, task-uncheck sounds.
    - Smooth animated transitions and hover effects.
    - Responsive layout — works on both mobile and desktop.
    - Sidebar breadcrumb: always shows "RU > B.Sc. (CA)".
    - Premium typography: Outfit + JetBrains Mono from Google Fonts.

13. PER-COURSE DATA ISOLATION  [NEW — April 2026]
    -------------------------------------------------
    - Each user's progress data is now stored separately per
      university and per course combination.
    - Storage key format:
        coursesync_<username>_<universityId>_<courseId>
    - This means the same user can maintain separate progress
      records for different courses in the future.

14. ADMIN PANEL (admin.html + admin.js)
    ------------------------------------
    - A separate admin page exists for resource management.
    - The admin can set topic-level YouTube and PDF links.
    - These links appear globally for all students.
    - Powered by Firebase Firestore.

================================================================
  FILE STRUCTURE
================================================================

  bca_tracker/
  └── app/
      ├── index.html          — Main app (login + dashboard + syllabus)
      ├── style.css           — All CSS styling (dark theme, glassmorphism)
      ├── app.js              — Core JavaScript logic
      ├── data.js             — All syllabus data (universitiesData hierarchy)
      ├── firebase-config.js  — Firebase SDK initialization
      ├── admin.html          — Admin panel UI
      ├── admin.js            — Admin panel logic
      └── PROJECT_INFO.txt    — This file. Project documentation.

================================================================
  FUTURE PLANS
================================================================

SHORT TERM (Priority):
-----------------------
  [ ] Add more universities:
      - Vinoba Bhave University (VBU), Hazaribagh
      - Sido Kanhu Murmu University (SKMU), Dumka
      - Jharkhand Raksha Shakti University
      - Nilamber Pitamber University
      - and more...

  [ ] Add more courses under Ranchi University:
      - BCA (Bachelor of Computer Application)
      - B.Sc. (IT)
      - B.Com (Computer Application)
      - MCA (Master of Computer Application)

  [ ] Upgrade Admin Panel:
      - Allow setting resources at the University and Course level.
      - Add topic search/filter functionality.

  [ ] Display topic descriptions in the syllabus view:
      - The "desc" field already exists in the data — just render it
        as an expandable section under each topic title.

  [ ] Exam Schedule feature:
      - Allow saving multiple exam dates per semester.
      - Show a multi-exam countdown on the dashboard.

MEDIUM TERM:
--------------
  [ ] Firebase Authentication:
      - Replace localStorage password storage with Firebase Auth
        (email/password) for proper security.

  [ ] Cloud Sync:
      - Sync user progress to Firebase Firestore so it is
        accessible across multiple devices.

  [ ] Topic Search Bar:
      - Allow students to search for any topic or subject by name
        across all semesters.

  [ ] Export Notes:
      - Allow users to export all their notes as a PDF or
        plain text file.

  [ ] Study Planner:
      - Let students create a weekly study schedule.
      - Choose which topics to study on which days.
      - Daily study reminders.

  [ ] Progressive Web App (PWA):
      - Make the app installable on mobile and desktop.
      - Add push notification support for study reminders.

LONG TERM (Big Vision):
------------------------
  [ ] National-level platform:
      - Cover syllabus for universities across all Indian states.
      - Support both CBCS and NEP 2020 curriculum formats.

  [ ] Community Features:
      - Students can share notes publicly.
      - Topic-level comment/discussion section.

  [ ] AI Study Assistant:
      - A RAG-based chatbot that answers questions about syllabus
        topics using the actual course material.
      - A "Shadow" AI persona (Gemini-based) has already been
        explored in a separate prototype.

  [ ] Practice Questions & Self-Assessment:
      - MCQ and short questions for each topic.
      - Auto-graded self-assessments.

  [ ] Leaderboard & Gamification:
      - Compare progress at college or university level.
      - Badges and achievement system to motivate study.

================================================================
  KNOWN ISSUES & CURRENT LIMITATIONS
================================================================

  - Only one university and one course is currently available
    (Ranchi University / B.Sc. CA). Adding new data requires
    manually editing data.js.

  - Passwords are stored as plain text in localStorage.
    This will be resolved when Firebase Authentication is
    implemented.

  - If the Firebase connection fails, global admin resources
    will not load. The app handles this gracefully by falling
    back to local data with a silent console warning.

  - Login state persists across browser refreshes by design
    (user is stored in localStorage). This is intended behavior.

  - The app is entirely client-side (no backend server).
    This is intentional for the current offline-first approach.

================================================================
  DEVELOPER INFO
================================================================

  Developer   : Aman Choudhary (OmtGod)
  GitHub      : https://github.com/Omtgod
  Instagram   : https://www.instagram.com/omt_god

  Built with love for students of Ranchi University
  and with the vision to expand to all Indian universities.

================================================================
