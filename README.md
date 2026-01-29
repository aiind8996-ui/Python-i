import tkinter as tk
from tkinter import messagebox, simpledialog
import hashlib
import platform
import time

# ================= LOGIN =================
def login_screen():
    login = tk.Tk()
    login.title("Login")
    login.geometry("300x200")
    login.configure(bg="black")

    tk.Label(login, text="HACKER LOGIN",
             fg="lime", bg="black",
             font=("Consolas", 14, "bold")).pack(pady=10)

    user = tk.Entry(login, bg="black", fg="lime", insertbackground="lime")
    user.pack(pady=5)

    pwd = tk.Entry(login, bg="black", fg="lime",
                   insertbackground="lime", show="*")
    pwd.pack(pady=5)

    def check():
        if user.get() == "admin" and pwd.get() == "1234":
            login.destroy()
            main_app()
        else:
            messagebox.showerror("Error", "Wrong Login")

    tk.Button(login, text="LOGIN", command=check,
              bg="black", fg="lime", width=15).pack(pady=10)

    login.mainloop()

# ================= MAIN APP =================
def main_app():
    app = tk.Tk()
    app.title("Ethical Hacker App")
    app.geometry("700x500")
    app.configure(bg="black")

    tk.Label(app, text="ETHICAL HACKER TERMINAL",
             fg="lime", bg="black",
             font=("Consolas", 18, "bold")).pack(pady=10)

    output = tk.Text(app, bg="black", fg="lime",
                     font=("Consolas", 10), height=15)
    output.pack(padx=10, pady=10)

    def log(msg):
        output.insert(tk.END, msg + "\n")
        output.see(tk.END)

    # ---------- FUNCTIONS ----------
    def password_check():
        p = simpledialog.askstring("Password", "Enter Password")
        if not p:
            return
        if len(p) >= 8:
            log("[+] Password Strong")
        else:
            log("[-] Password Weak")

    def hash_text():
        t = simpledialog.askstring("Hash", "Enter Text")
        if not t:
            return
        h = hashlib.sha256(t.encode()).hexdigest()
        log("[HASH] " + h)

    def system_info():
        log("OS : " + platform.system())
        log("Machine : " + platform.machine())
        log("Processor : " + platform.processor())

    def fake_hack():
        log("[*] Connecting...")
        app.update()
        time.sleep(0.5)
        log("[*] Bypassing firewall...")
        app.update()
        time.sleep(0.5)
        log("[✓] Simulation Complete (Learning Mode)")

    def clear():
        output.delete("1.0", tk.END)

    # ---------- BUTTONS ----------
    frame = tk.Frame(app, bg="black")
    frame.pack()

    def btn(text, cmd, r, c):
        tk.Button(frame, text=text, command=cmd,
                  bg="black", fg="lime",
                  width=25).grid(row=r, column=c, padx=5, pady=5)

    btn("Password Check", password_check, 0, 0)
    btn("Hash Generator", hash_text, 0, 1)
    btn("System Info", system_info, 1, 0)
    btn("Hack Simulation", fake_hack, 1, 1)
    btn("Clear Screen", clear, 2, 0)

    app.mainloop()

# ================= START =================
login_screen()
