import tkinter as tk
from tkinter import filedialog, simpledialog
import webbrowser
import os

# ================= MAIN APP =================
app = tk.Tk()
app.title("Mini YouTube")
app.geometry("800x500")
app.configure(bg="white")

# ================= HEADER =================
header = tk.Frame(app, bg="red", height=50)
header.pack(fill="x")

tk.Label(
    header,
    text="Mini YouTube",
    bg="red",
    fg="white",
    font=("Arial", 16, "bold")
).pack(side="left", padx=10)

# ================= SEARCH =================
def search_video():
    q = search_entry.get()
    if q.strip() == "":
        return
    url = f"https://www.youtube.com/results?search_query={q}"
    webbrowser.open(url)

search_entry = tk.Entry(header, width=40, font=("Arial", 12))
search_entry.pack(side="left", padx=10)

tk.Button(
    header,
    text="Search",
    command=search_video,
    bg="white"
).pack(side="left")

# ================= BODY =================
body = tk.Frame(app, bg="#f5f5f5")
body.pack(fill="both", expand=True)

output = tk.Text(body, height=15)
output.pack(padx=10, pady=10, fill="both", expand=True)

def log(msg):
    output.insert(tk.END, msg + "\n")
    output.see(tk.END)

# ================= FUNCTIONS =================
def open_youtube():
    webbrowser.open("https://www.youtube.com")
    log("Opened YouTube Homepage")

def play_video_url():
    url = simpledialog.askstring("Play Video", "Enter YouTube Video URL")
    if url:
        webbrowser.open(url)
        log("Playing video from URL")

def play_local_video():
    file = filedialog.askopenfilename(
        filetypes=[("Video Files", "*.mp4 *.mkv *.avi")]
    )
    if file:
        os.startfile(file)
        log("Playing local video")

# ================= BUTTONS =================
btn_frame = tk.Frame(app, bg="#f5f5f5")
btn_frame.pack(pady=10)

tk.Button(btn_frame, text="Open YouTube", width=20, command=open_youtube).grid(row=0, column=0, padx=5)
tk.Button(btn_frame, text="Play Video URL", width=20, command=play_video_url).grid(row=0, column=1, padx=5)
tk.Button(btn_frame, text="Play Local Video", width=20, command=play_local_video).grid(row=0, column=2, padx=5)

app.mainloop()
