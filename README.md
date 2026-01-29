# Python-i
import tkinter as tk
from tkinter import messagebox, filedialog
import hashlib
import socket
import os
import platform
from cryptography.fernet import Fernet

# ================== MAIN WINDOW ==================
app = tk.Tk()
app.title("⚡ ETHICAL HACKER TOOL ⚡")
app.geometry("700x550")
app.configure(bg="black")

# ================== TITLE ==================
title = tk.Label(
    app,
    text="ETHICAL HACKER CONTROL PANEL",
    fg="lime",
    bg="black",
    font=("Consolas", 18, "bold")
)
title.pack(pady=10)

output = tk.Text(app, height=12, bg="black", fg="lime", font=("Consolas", 10))
output.pack(pady=10)

# ================== FUNCTIONS ==================

def password_check():
    pwd = filedialog.askstring("Password", "Enter Password")
    if not pwd:
        return
    strength = "WEAK"
    if len(pwd) >= 8 and any(c.isdigit() for c in pwd) and any(c.isupper() for c in pwd):
        strength = "STRONG"
    output.insert(tk.END, f"[+] Password Strength: {strength}\n")

def generate_hash():
    text = filedialog.askstring("Hash", "Enter Text")
    if not text:
        return
    hashed = hashlib.sha256(text.encode()).hexdigest()
    output.insert(tk.END, f"[+] SHA256 Hash:\n{hashed}\n")

def port_scan():
    output.insert(tk.END, "[*] Scanning localhost ports...\n")
    for port in range(75, 85):
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(0.2)
        result = s.connect_ex(("127.0.0.1", port))
        if result == 0:
            output.insert(tk.END, f"[OPEN] Port {port}\n")
        s.close()
    output.insert(tk.END, "[✓] Scan Complete\n")

def system_info():
    info = f"""
OS: {platform.system()}
Version: {platform.version()}
Machine: {platform.machine()}
Processor: {platform.processor()}
"""
    output.insert(tk.END, info + "\n")

# Encryption demo
key = Fernet.generate_key()
cipher = Fernet(key)

def encrypt_file():
    file = filedialog.askopenfilename()
    if not file:
        return
    with open(file, "rb") as f:
        data = f.read()
    encrypted = cipher.encrypt(data)
    with open(file + ".enc", "wb") as f:
        f.write(encrypted)
    output.insert(tk.END, "[✓] File Encrypted\n")

def decrypt_file():
    file = filedialog.askopenfilename()
    if not file:
        return
    with open(file, "rb") as f:
        data = f.read()
    decrypted = cipher.decrypt(data)
    new_file = file.replace(".enc", "")
    with open(new_file, "wb") as f:
        f.write(decrypted)
    output.insert(tk.END, "[✓] File Decrypted\n")

# ================== BUTTONS ==================
btn_frame = tk.Frame(app, bg="black")
btn_frame.pack()

def hacker_button(text, cmd):
    return tk.Button(
        btn_frame,
        text=text,
        command=cmd,
        bg="black",
        fg="lime",
        font=("Consolas", 10, "bold"),
        width=25,
        relief="groove",
        border=2
    )

hacker_button("🔐 Password Strength Check", password_check).grid(row=0, column=0, padx=5, pady=5)
hacker_button("🧬 Generate SHA256 Hash", generate_hash).grid(row=0, column=1, padx=5, pady=5)
hacker_button("🌐 Localhost Port Scan", port_scan).grid(row=1, column=0, padx=5, pady=5)
hacker_button("💻 System Information", system_info).grid(row=1, column=1, padx=5, pady=5)
hacker_button("📁 Encrypt File", encrypt_file).grid(row=2, column=0, padx=5, pady=5)
hacker_button("📂 Decrypt File", decrypt_file).grid(row=2, column=1, padx=5, pady=5)

# ================== RUN ==================
app.mainloop()
