# 🍅 Pomodoro Timer

This Python program is a GUI-based Pomodoro Timer built using the `tkinter` library. The timer follows the Pomodoro technique, which alternates between work and break intervals to improve focus and productivity.

## 📂 Project Structure

- **Timer Constants**: Customizable intervals for work, short breaks, and long breaks.
- **Timer Reset**: Functionality to reset the timer and restart the session.
- **Timer Mechanism**: Controls the switching between work and break intervals.
- **Countdown Mechanism**: Handles the countdown display and transitions between work and break sessions.
- **UI Setup**: GUI setup using `tkinter` with labels, buttons, and a canvas with a tomato icon for visual appeal.

---

## 🔧 Code Explanation

### 1. Constants and Initial Setup
```python
PINK = "#e2979c"
RED = "#e7305b"
GREEN = "#9bdeac"
YELLOW = "#f7f5dd"
FONT_NAME = "Courier"
WORK_MIN = 25
SHORT_BREAK_MIN = 5
LONG_BREAK_MIN = 20
reps = 0
timer = None
```
- Color Constants: Colors for different labels and stages of the timer.
- Interval Constants: Time durations for work, short break, and long break intervals.

### 2. Timer Reset
```python
def resetTimer():
    window.after_cancel(timer)
    canvas.itemconfig(timerText, text="00:00")
    titleLabel.config(text="Timer")
    checkMark.config(text="")
    global reps
    reps = 0
```
- Resets the timer, clears the checkmarks, and sets the display back to 00:00.

### 3. Timer Mechanism
```python
def startTimer():
    global reps
    reps += 1
    workSec = WORK_MIN * 60
    shortBreakSec = SHORT_BREAK_MIN * 60
    longBreakSec = LONG_BREAK_MIN * 60

    if reps % 8 == 0:
        countDown(longBreakSec)
        titleLabel.config(text="Break", fg=RED)
    elif reps % 2 == 0:
        countDown(shortBreakSec)
        titleLabel.config(text="Break", fg=PINK)
    else:
        countDown(workSec)
        titleLabel.config(text="Work", fg=GREEN)
```
- Controls the intervals based on the Pomodoro cycle, alternating between work and break sessions.

### 4. Countdown Mechanism
```python
def countDown(count):
    countMin = math.floor(count / 60)
    countSec = count % 60
    if countSec < 10:
        countSec = f"0{countSec}"

    canvas.itemconfig(timerText, text=f"{countMin}:{countSec}")
    if count > 0:
        global timer
        timer = window.after(1000, countDown, count - 1)
    else:
        startTimer()
        mark = ""
        workSessions = math.floor(reps / 2)
        for _ in range(workSessions):
            mark += "✅"
        checkMark.config(text=mark)
```
- Countdown Display: Updates the timer every second and switches to the next interval when the countdown reaches zero.
- Checkmarks: Adds a checkmark for each completed work session.

### 5. UI Setup
```python
window = Tk()
window.title("Pomodoro")
window.config(padx=100, pady=50, bg=YELLOW)

titleLabel = Label(text="Timer", fg=GREEN, font=(FONT_NAME, 50), bg=YELLOW)
titleLabel.grid(column=1, row=0)

canvas = Canvas(width=200, height=224, bg=YELLOW, highlightthickness=0)
tomatoImg = PhotoImage(file="tomato.png")
canvas.create_image(100, 110, image=tomatoImg)
timerText = canvas.create_text(100, 130, text="00:00", fill="white", font=(FONT_NAME, 35, "bold"))
canvas.grid(column=1, row=1)

startButton = Button(text="Start", highlightthickness=0, command=startTimer)
startButton.grid(column=0, row=2)

resetButton = Button(text="Reset", highlightthickness=0, command=resetTimer)
resetButton.grid(column=2, row=2)

checkMark = Label(fg=GREEN, bg=YELLOW)
checkMark.grid(column=1, row=3)

window.mainloop()
```
- GUI Elements: Includes labels, buttons, and a canvas with a tomato image representing the Pomodoro technique.
- Grid Layout: Organizes the components in a structured format.

## 🚀 How to Run
1. Ensure Python and tkinter are installed on your system.
2. Save an image named tomato.png in the same directory as the script.
3. Run the program:
```bash
python script_name.py
```
4. Click "Start" to begin the timer, and "Reset" to reset it.

## 📝 Notes
- Adjustable Intervals: Customize WORK_MIN, SHORT_BREAK_MIN, and LONG_BREAK_MIN for different durations.
- Timer Sequence: The timer follows a sequence of work (25 min), short break (5 min), and a long break after 4 work sessions.

## 🔗 Additional Resources
- Pomodoro Technique
- tkinter Documentation
