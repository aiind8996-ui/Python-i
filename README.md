import tkinter as tk
from tkinter import messagebox, filedialog, simpledialog
import hashlib
import socket
import platform
import time
from cryptography.fernet import Fernet

# =========================
# LOGIN WINDOW
# =========================
def start_login():
    login = tk.Tk()
    login.title("SECURE LOGIN")
    login.geometry("320x220")
    login.configure(bg="black")

    tk.Label(login, text="ETHICAL HACKER LOGIN",
             fg="lime", bg="black",
             font=("Consolas", 14, "bold")).pack(pady=15)

    user = tk.Entry(login, bg="black", fg="lime",
                    insertbackground="lime", justify="center")
    user.insert(0, "admin")
    user.pack(pady=5)

    pwd = tk.Entry(login, bg="black", fg="lime",
                   insertbackground="lime", show="*", justify="center")
    pwd.pack(pady=5)

    def check_login():
        if user.get() == "admin" and pwd.get() == "admin123":
            login.destroy()
            main_app()
        else:
            messagebox.showerror("ACCESS DENIED", "Wrong Username or Password")

    tk.Button(login, text="ACCESS SYSTEM",
              command=check_login,
              bg="black", fg="lime",
              width=18).pack(pady=15)

    login.mainloop()

# =========================
# MAIN APPLICATION
# =========================
def main_app():
    app = tk.Tk()
    app.title("⚡ ETHICAL HACKER CONTROL PANEL ⚡")
    app.geometry("900x600")
    app.configure(bg="black")

    tk.Label(app, text="ETHICAL HACKER TERMINAL",
             fg="lime", bg="black",
             font=("Consolas", 20, "bold")).pack(pady=10)

    output = tk.Text(app, bg="black", fg="lime",
                     font=("Consolas", 10))
    output.pack(fill="both", expand=True, padx=10, pady=10)

    def log(text):
        output.insert(tk.END, text + "\n")
        output.see(tk.END)

    # ================= FUNCTIONS =================

    def password_strength():
        pwd = simpledialog.askstring("Password Check", "Enter Password")
        if not pwd:
            return
        score = 0
        if len(pwd) >= 8: score += 1
        if any(c.isupper() for c in pwd): score += 1
        if any(c.isdigit() for c in pwd): score += 1
        if any(c in "!@#$%^&*" for c in pwd): score += 1

        strength = ["VERY WEAK", "WEAK", "MEDIUM", "STRONG", "VERY STRONG"][score]
        log(f"[+] Password Strength: {strength}")

    def generate_hash():
        text = simpledialog.askstring("Hash Generator", "Enter Text")
        if not text:
            return
        log("[SHA256] " + hashlib.sha256(text.encode()).hexdigest())
        log("[MD5]    " + hashlib.md5(text.encode()).hexdigest())

    def port_scan():
        log("[*] Starting Localhost Port Scan...")
        for port in range(70, 100):
            s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            s.settimeout(0.15)
            if s.connect_ex(("127.0.0.1", port)) == 0:
                log(f"[OPEN] Port {port}")
            s.close()
        log("[✓] Port Scan Finished")

    def system_info():
        log("---- SYSTEM INFO ----")
        log("OS        : " + platform.system())
        log("Version   : " + platform.version())
        log("Machine   : " + platform.machine())
        log("Processor : " + platform.processor())

    key = Fernet.generate_key()
    cipher = Fernet(key)

    def encrypt_file():
        file = filedialog.askopenfilename()
        if not file:
            return
        data = open(file, "rb").read()
        open(file + ".enc", "wb").write(cipher.encrypt(data))
        log("[✓] File Encrypted Successfully")

    def decrypt_file():
        file = filedialog.askopenfilename()
        if not file:
            return
        data = open(file, "rb").read()
        open(file.replace(".enc", ""), "wb").write(cipher.decrypt(data))
        log("[✓] File Decrypted Successfully")

    def attack_simulation():
        log("[*] Training Attack Simulation Started")
        for i in range(1, 6):
            log(f"Injecting Payload {i}/5 ...")
            app.update()
            time.sleep(0.4)
        log("[✓] Simulation Completed (Training Mode Only)")

    def clear_logs():
        output.delete("1.0", tk.END)

    # ================= BUTTON PANEL =================
    panel = tk.Frame(app, bg="black")
    panel.pack(pady=10)

    def btn(text, cmd, r, c):
        tk.Button(panel, text=text, command=cmd,
                  bg="black", fg="lime",
                  width=28, height=2,
                  font=("Consolas", 10, "bold"),
                  relief="groove", border=2)\
            .grid(row=r, column=c, padx=5, pady=5)

    btn("🔐 Password Strength Checker", password_strength, 0, 0)
    btn("🧬 Hash Generator", generate_hash, 0, 1)
    btn("🌐 Localhost Port Scanner", port_scan, 1, 0)
    btn("💻 System Information", system_info, 1, 1)
    btn("📁 Encrypt File", encrypt_file, 2, 0)
    btn("📂 Decrypt File", decrypt_file, 2, 1)
    btn("⚠️ Attack Simulation (Demo)", attack_simulation, 3, 0)
    btn("🧹 Clear Terminal", clear_logs, 3, 1)

    app.mainloop()

# =========================
# START PROGRAM
# =========================
start_login()
