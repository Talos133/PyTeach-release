# PyTeach — User Guide

**Version:** 0.1.0  
**Audience:** QA engineers, developers, technical support, integrators

---

## Table of Contents

1. [About PyTeach](#1-about-pyteach)
2. [Profiles & Login](#2-profiles--login)
3. [The Study Area](#3-the-study-area)
4. [Lesson Narration](#4-lesson-narration)
5. [The Code Panel](#5-the-code-panel)
6. [Quizzes & Knowledge Checks](#6-quizzes--knowledge-checks)
7. [Accessibility](#7-accessibility)
8. [External Resources](#8-external-resources)
9. [Status Screen](#9-status-screen)
10. [Settings](#10-settings)
11. [Switching Profiles & Logging Out](#11-switching-profiles--logging-out)
12. [Course Completion](#12-course-completion)

---

## 1. About PyTeach

### What it is

PyTeach is a desktop application for learning Python test automation from scratch. It delivers 70 structured lessons across 8 modules — from Python fundamentals and OOP to pytest, API testing, Playwright UI testing, database testing, mobile testing with Appium, and professional tooling — entirely offline, with no internet connection or cloud subscription required.

Every lesson comes with:
- Structured written content with examples
- A live Python code editor (Monaco, the same engine as VS Code) with a sandboxed execution environment
- A five-question knowledge check quiz

### Who it is for

PyTeach is aimed at people who already work in software — QA engineers, developers moving into test automation, technical support staff, and integrators — and who want to build a solid, hands-on Python test automation skill set. The course assumes basic comfort with technology and scripting concepts, but no prior Python knowledge.

### What problems it solves

Learning test automation in the traditional way involves a significant amount of environment setup friction before a student writes a single line of test code: installing Python, configuring a virtual environment, picking an IDE, setting up a framework, finding exercises to run against, and managing external service dependencies. For teams onboarding new testers or running self-directed training programmes, this overhead is a real barrier.

PyTeach eliminates that friction entirely:

- **No setup required.** Python, all frameworks, and all dependencies ship inside the app. There is nothing to install beyond PyTeach itself.
- **No internet dependency.** All lesson content, code execution, and exercise servers (REST API mock, Appium mock) run locally. Students can learn on a plane with no Wi-Fi.
- **Exercises run against real-looking infrastructure.** The bundled mock API server implements JWT, API key, and Basic auth; returns realistic payloads; and supports error injection. The mock Appium server implements the W3C WebDriver protocol subset. Students write real production-quality test code, not toy examples.
- **Progress is per-profile.** Multiple people can use the same installation with separate profiles and separate progress tracking.

---

## 2. Profiles & Login

### The Profile Picker

When PyTeach opens you land on the **Profile Picker** — a screen listing all student profiles stored on this machine. If this is a fresh installation, the list will be empty.

The app may show a brief "Starting up…" message while the backend initialises. This is normal and takes up to 10–12 seconds on first launch (see the installation README for details). The profile list appears automatically once the backend is ready.

### Creating a New Profile

1. Click **New Profile** on the Profile Picker screen.
2. Enter a name. This is display-only and can be anything — first name, username, whatever makes sense for your context.
3. Choose a 4-digit PIN. This PIN is required every time you log in with this profile. It is stored locally (hashed) and cannot be recovered if forgotten — in that case the profile will need to be deleted and recreated.
4. Click **Create**. You are taken directly into the Study Area.

### Logging In to an Existing Profile

1. Click your name on the Profile Picker.
2. Enter your 4-digit PIN.
3. Click **Unlock** (or press Enter).

**Wrong PIN:** After 3 incorrect attempts in a single session, the profile is temporarily locked. Close and reopen the app to reset the lockout counter.

### Multiple Profiles

Each profile is fully independent — separate lesson progress, separate quiz history, separate code workspace, and separate settings. This makes PyTeach suitable for shared workstations where multiple team members are going through the course at different paces.

---

## 3. The Study Area

Once logged in, you enter the **Study Area** — the main learning environment. It has three parts: the sidebar, the lesson panel, and the code panel.

### The Sidebar

The left sidebar lists all course modules as collapsible groups. Each module expands to show its lessons. Lessons are unlocked sequentially — you must complete a lesson's quiz before the next lesson becomes accessible. Locked lessons show a padlock icon. The currently active lesson is highlighted.

### The Lesson Panel

The main area shows the current lesson. The top of the panel displays:

- **Breadcrumb** — module number, lesson number, and total lesson count ("Module 1 › Lesson 3 of 13")
- **Part indicator** — if the lesson is split into multiple sub-pages, a pill shows "Part 2 of 4"
- **Lesson title and subtitle**
- **Narration controls** — play/pause/stop and speed chips (see [Section 4](#4-lesson-narration))
- **Progress bar** — shows how far through the overall course you are

Below the header, the lesson content scrolls freely. At the bottom of the panel is the navigation footer.

### Sub-pages (Multi-part Lessons)

Longer lessons are split into sub-pages to keep each screen focused and readable. Navigate between them using:

- **Next part →** button (bottom right)
- **← Previous part** button (bottom left, when applicable)
- **Dot navigation** (centre of the footer) — click any dot to jump directly to that sub-page

The quiz is only accessible from the final sub-page of a lesson.

### Getting Back to the Profile Picker

Click the **power/logout icon** (⏻) in the top-right navigation bar from anywhere in the Study Area. You will be returned to the Profile Picker. Your progress is saved automatically — there is no manual save step.

### The Top Navigation Bar

The top bar is always visible and provides access to:

| Icon | Destination |
|---|---|
| 📚 | Resources screen (external links and references) |
| ⚙️ | Settings panel |
| Profile avatar | Displays current profile name |
| ⏻ | Log out / return to Profile Picker |

---

## 4. Lesson Narration

PyTeach can read lesson content aloud using your operating system's built-in text-to-speech engine — no internet connection or additional service required.

The narration controls appear in the lesson header, below the title, on every sub-page that has readable prose content. Pages that consist entirely of code or reference tables do not show the narration bar.

### Controls

| Control | Action |
|---|---|
| **▶ Narrate** | Start reading from the beginning of the current sub-page |
| **⏸ Pause** | Pause mid-sentence; click again (now showing **Resume**) to continue |
| **⏹** (stop square) | Stop and reset to the beginning |
| **0.5× 0.75× 1× 1.25× 1.5× 2×** | Speed chips — click to change reading speed |

The active speed chip is highlighted in the accent colour. Changing speed while narration is playing restarts from the current paragraph at the new rate. Changing speed while paused updates the rate for the next time you press play.

### What Gets Read (and What Doesn't)

The narration engine extracts plain prose from the lesson markdown before reading. The following are **skipped**:

- Code blocks (fenced ` ``` ` blocks)
- Inline code snippets
- Tables
- Tip, info, and try-it callout boxes
- Horizontal dividers

Headings are read as plain text (the `#` markers are stripped). Bold and italic markers are stripped but the text they wrap is read normally.

### Narration and Sub-page Navigation

Narration **stops at the end of the current sub-page** and returns to idle. It does not auto-advance to the next sub-page. This is intentional — students often need to run code or follow an exercise before moving on, and the narration waiting for them makes that workflow comfortable.

When you navigate to a different sub-page or a different lesson, any in-progress narration stops automatically.

### Platform Notes

| Platform | Status |
|---|---|
| macOS | Works out of the box (uses system Siri/Alex voices) |
| Windows | Works out of the box (uses Microsoft voices) |
| Linux | Requires `speech-dispatcher` — see the [installation README](./README.md#linux-narration-text-to-speech) |

---

## 5. The Code Panel

The **Code Panel** is a full Monaco editor (the same engine that powers VS Code) embedded in the right side of the Study Area. It opens automatically when a lesson reaches a code reference point, or you can open it manually at any time.

### Opening and Closing

- Click the **{ } Code** button in the top-right corner of the lesson panel header.
- The panel opens alongside the lesson content.
- Click **✕ Close** in the code panel header to close it.

When the panel is auto-opened by a lesson (marked with an ⚡ AUTO badge), it means the code example referenced in the lesson is now in view. The editor will contain the relevant starter code or exercise scaffold.

### The Editor

The editor is a standard Python editor with:

- Syntax highlighting
- Line numbers
- 4-space indentation
- Word wrap
- Auto layout — resizes cleanly when the panel is opened or closed

You can edit the code freely. Changes are not persisted across lesson navigation — the editor resets to the starter code when you move to a different lesson. If you want to keep your work, copy it out before navigating away.

### Running Code

Click **▶ Run** to execute the entire file. Output streams into the output panel below the editor in real time.

**Running a Selection:**  
Highlight any lines in the editor and the button label changes to **▶ Run Selection**. Only the selected lines are sent to the Python runtime. Deselect to go back to running the full file.

**Keyboard shortcut:**  
`⌘ Return` on macOS / `Ctrl+Return` on Windows and Linux.  
The shortcut runs selected code if there is a selection, or the full file if there is not. It is only active when the editor is focused.

### Stopping Execution

Click **■ Stop** (visible while code is running) to terminate the script immediately. This is useful if you accidentally trigger an infinite loop or a long-running network call.

A timeout is also enforced per module — if a script runs longer than the module's allowed time (which scales with lesson complexity), it is stopped automatically with a timeout message in the output.

### Output Panel

The output panel below the editor shows:

- `stdout` and `stderr` from the executed Python code, streamed line by line as the script runs
- Timeout messages if the script is stopped for exceeding the time limit
- A placeholder message (`# Run your code to see output here`) when no code has been run yet in this session

The output panel uses a read-only Monaco editor with monospace formatting, making it easy to read structured output, JSON payloads, and pytest results.

### Compare with Solution

After you run your code at least once (and if the lesson has a model solution), a **Compare with Solution** button appears next to the Run button. Clicking it reveals a third read-only panel below the output showing the model solution.

This is intentionally gated behind a first run — the idea is that you attempt the exercise before seeing the answer. The solution panel is for cross-checking your approach, not for copying.

---

## 6. Quizzes & Knowledge Checks

Every lesson (except a small number of read-only reference lessons) ends with a five-question multiple-choice quiz. Completing the quiz is what unlocks the next lesson.

### Taking the Quiz

On the final sub-page of a lesson, you will see a **"Ready for the knowledge check?"** teaser block at the bottom of the content. Click **Take Quiz →** in the lesson footer to enter quiz mode.

In quiz mode, each question is presented with four answer options. Select one option per question by clicking the radio button or the answer text. All five questions are shown on screen at once — you can scroll through them and change your answers before submitting.

The **Submit Quiz** button at the bottom activates only when every question has been answered.

### Submitting

Click **Submit Quiz**. Answers are evaluated server-side — the correct answers are never sent to the browser, which means a student cannot inspect the page source to cheat. Results come back immediately.

### Reading Results

After submission:

- **Correct answers** highlight in green.
- **Wrong answers** highlight in red with a strikethrough on your chosen option.
- The correct answer for any wrong question is highlighted in green regardless.
- An **Explanation** block expands below each wrong answer, describing why the correct answer is right and, where relevant, showing a code snippet to illustrate the concept.

A score summary appears at the bottom:
- **Perfect score** (5/5) — green banner
- **Good work** (4/5, ≥70%) — green banner
- **Needs review** (<70%) — neutral banner with an encouragement message

### Reviewing a Completed Quiz

Once submitted, quizzes cannot be re-taken. You can review your results at any time by navigating back to the lesson and clicking **Review Quiz →** in the lesson footer. The quiz opens in read-only mode showing all your answers and the correct answers.

### Cross-Checking with the Code Panel

For lessons with coding exercises, the **Compare with Solution** button in the code panel (available after your first code run) lets you compare your implementation against the model solution. This is separate from the quiz — it is about verifying your code, not your conceptual understanding.

---

## 7. Accessibility

PyTeach includes a set of accessibility features accessible via the **Settings** panel (⚙️ in the top navigation bar).

### Colour Palette

The app ships with multiple colour themes. Cycle through them in Settings under **Appearance → Colour Palette**. All themes are designed for the dark environment typical of developer workstations, with varying contrast levels.

### Font Size

Adjust the lesson content font size under **Appearance → Font Size**. Changes apply immediately across all lesson content.

### Narration

The lesson narration feature (see [Section 4](#4-lesson-narration)) functions as a full accessibility aid for students who benefit from audio alongside text. It uses the OS native voice engine — no third-party screen reader integration is required.

---

## 8. External Resources

The **Resources screen** (📚 icon in the top navigation bar) provides a curated list of external reference links relevant to the course content — official Python docs, Playwright documentation, pytest documentation, and similar.

Links open in your **system default browser**, not inside PyTeach. This is intentional — reference documentation is more useful in a full browser with bookmarking, tabs, and browser history.

The Resources screen is accessible from anywhere in the app and does not affect your lesson progress or navigation state.

---

## 9. Status Screen

The **Status screen** gives you a full picture of a student's progress through the course. Access it from the ⚙️ Settings icon or from the top navigation bar.

It displays:

- **Module progress** — lesson completion status per module, shown as a grid
- **Overall completion percentage**
- **Study streak** — consecutive days with at least one lesson completed, shown with an animated flame indicator
- **Quiz accuracy** — overall percentage of quiz questions answered correctly across all submitted quizzes

The Status screen is useful for support staff checking in on a student's progress, or for integrators who want a quick summary view without diving into individual lessons.

---

## 10. Settings

Access Settings via the **⚙️ icon** in the top navigation bar.

### Appearance

| Setting | Description |
|---|---|
| Colour Palette | Switch between available colour themes |
| Font Size | Adjust lesson content text size |

### Browser Preference

Relevant from Module 4 (Playwright) onwards. Choose between **Chromium** (default, available on all platforms) and **WebKit** (macOS only, uses the system engine and is significantly lighter). This setting controls which browser Playwright uses when running browser automation exercises inside PyTeach.

WebKit is only shown as an option on macOS after the student reaches Module 4.

### Danger Zone — Delete Profile

The Settings panel includes a **Delete Profile** section at the bottom. Deleting a profile permanently removes all progress, quiz history, and the associated code workspace for that student.

To confirm deletion, you must type the profile name exactly as it appears. After deletion, the app returns to the Profile Picker automatically.

**This action is irreversible.** There is no confirmation email, backup, or recovery path.

---

## 11. Switching Profiles & Logging Out

To return to the Profile Picker from anywhere in the app:

- Click the **⏻ (power/logout icon)** in the top-right corner of the navigation bar.

You are taken immediately to the Profile Picker. All progress is saved automatically — there is no manual save or sync step, and nothing is lost by logging out mid-lesson.

From the Profile Picker, any profile on the machine can be selected. This makes it straightforward for a single machine to serve multiple students or for a technical support person to check in on a specific profile's state.

---

## 12. Course Completion

When a student submits the quiz for the final lesson in the course (Module 8, Lesson 3), PyTeach automatically navigates to the **Course Complete** screen.

The screen includes:

- A confetti animation
- A personalised certificate displaying the student's profile name
- Total quiz accuracy across all 70 lessons
- A **Print / Save as PDF** button for printing or archiving the certificate

After viewing the Course Complete screen, the student can still access all lessons and quizzes in read-only review mode. The course is considered complete; no further unlocking is required.

The Course Complete screen is also accessible at any time after completion via the Status screen.
