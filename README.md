import time
import tkinter as tk
import math

def update_analog_clock():
    canvas.delete("hands")  # Remove old hands

    # Get current time
    now = time.localtime()
    hour = now.tm_hour % 12
    minute = now.tm_min
    second = now.tm_sec

    # Angles for hands
    sec_angle = math.radians(second * 6)
    min_angle = math.radians(minute * 6)
    hour_angle = math.radians((hour * 30) + (minute / 2))

    # Calculate hand coordinates
    sec_x = center + 90 * math.sin(sec_angle)
    sec_y = center - 90 * math.cos(sec_angle)

    min_x = center + 70 * math.sin(min_angle)
    min_y = center - 70 * math.cos(min_angle)

    hour_x = center + 50 * math.sin(hour_angle)
    hour_y = center - 50 * math.cos(hour_angle)

    # Draw hands
    canvas.create_line(center, center, sec_x, sec_y, width=1, fill='red', tags="hands")
    canvas.create_line(center, center, min_x, min_y, width=3, fill='blue', tags="hands")
    canvas.create_line(center, center, hour_x, hour_y, width=5, fill='black', tags="hands")

    # Redraw every 1000ms
    root.after(1000, update_analog_clock)

def update_time_and_greeting():
    current_time = time.strftime('%H:%M:%S')
    hour = int(time.strftime('%H'))

    time_label.config(text="Current Time: " + current_time)

    if 5 <= hour < 12:
        greeting = "Good Morning!"
    elif 12 <= hour < 18:
        greeting = "Good Afternoon!"
    elif 18 <= hour < 22:
        greeting = "Good Evening!"
    else:
        greeting = "Good Night!"

    greeting_label.config(text=greeting)

    root.after(1000, update_time_and_greeting)

# GUI Setup
root = tk.Tk()
root.title("Analog Clock with Greeting")
root.geometry("400x500")
root.configure(bg="#222831")  # dark background


# Greeting label
greeting_label = tk.Label(root, text="", font=("Arial", 16), bg="#f0f0f0", fg="green")
greeting_label.pack(pady=10)

# Digital time label
time_label = tk.Label(root, text="", font=("Arial", 14), bg="#f0f0f0", fg="blue")
time_label.pack(pady=5)

# Analog Clock Canvas
canvas = tk.Canvas(root, width=300, height=300, bg="white")
canvas.pack(pady=10)
center = 150

# Draw the clock dial
for i in range(12):
    angle = math.radians(i * 30)
    x = center + 100 * math.sin(angle)
    y = center - 100 * math.cos(angle)
    canvas.create_text(x, y, text=str(i if i != 0 else 12), font=("Helvetica", 12, "bold"), fill="#333")

canvas.create_oval(center-110, center-110, center+110, center+110, outline="#eeeeee", width=6)
canvas.configure(bg="#f8fafa")  # light background

# Start functions
update_time_and_greeting()
update_analog_clock()
root.mainloop()


    
