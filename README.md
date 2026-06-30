# RSA
import math
import tkinter as tk
from tkinter import messagebox

# ---------------
# мат логика RSA
# ---------------

def is_prime(n):
    if n < 2: return False
    for i in range(2, int(math.isqrt(n)) + 1):
        if n % i == 0: return False
    return True

def encrypt_message(text, e, n):
    return [pow(ord(char), e, n) for char in text]

def decrypt_message(encrypted_data, d, n):
    return "".join([chr(pow(char, d, n)) for char in encrypted_data])

# -----------------------
# Логика интерфейса
# -----------------------

def run_rsa():
    try:
        p = int(entry_p.get())
        q = int(entry_q.get())
        e = int(entry_e.get())
        text = entry_msg.get()

        if not (is_prime(p) and is_prime(q)):
            messagebox.showerror("Ошибка", "P и Q должны быть простыми числами!")
            return

        n = p * q
        phi = (p - 1) * (q - 1)

        if math.gcd(e, phi) != 1:
            messagebox.showerror("Ошибка", f"e должно быть взаимно простым с {phi}!")
            return

        d = pow(e, -1, phi)

        encrypted = encrypt_message(text, e, n)
        decrypted = decrypt_message(encrypted, d, n)

        label_n.config(text=f"n = {n}")
        label_d.config(text=f"d = {d}")
        entry_enc.delete(0, tk.END)
        entry_enc.insert(0, str(encrypted))
        entry_dec.delete(0, tk.END)
        entry_dec.insert(0, decrypted)

    except ValueError:
        messagebox.showerror("Ошибка", "Введите корректные числовые значения!")
#------------
# GUI
#------------
root = tk.Tk()
root.title("RSA Учебный проект")
root.geometry("400x500")

tk.Label(root, text="P:").pack();
entry_p = tk.Entry(root);
entry_p.pack()
tk.Label(root, text="Q:").pack();
entry_q = tk.Entry(root);
entry_q.pack()
tk.Label(root, text="E:").pack();
entry_e = tk.Entry(root);
entry_e.pack()
tk.Label(root, text="Сообщение:").pack();
entry_msg = tk.Entry(root);
entry_msg.pack()

tk.Button(root, text="Зашифровать", command=run_rsa).pack(pady=10)

label_n = tk.Label(root, text="n = ");
label_n.pack()
label_d = tk.Label(root, text="d = ");
label_d.pack()
tk.Label(root, text="Шифротекст:").pack()
entry_enc = tk.Entry(root, width=50);
entry_enc.pack()
tk.Label(root, text="Расшифровка:").pack()
entry_dec = tk.Entry(root, width=50);
entry_dec.pack()

root.mainloop()
