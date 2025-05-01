# An-extended-Hadamard-Decoder

import tkinter as tk
from tkinter import messagebox

# === Step 1: Generate Hadamard Matrix ===
def generate_hadamard_matrix(n):
    if n == 1:
        return [[1]]
    H = generate_hadamard_matrix(n // 2)
    top = [row + row for row in H]
    bottom = [row + [-x for x in row] for row in H]
    return top + bottom

# === Step 2: Decode Function ===
def decode():
    try:
        r = int(r_entry.get())
        n = 2 ** r

        # Generate Hadamard matrix
        H = generate_hadamard_matrix(n)

        # Step 3: Get corrupted codeword input
        raw_input = codeword_entry.get()
        received = list(map(int, raw_input.strip().split(',')))

        if len(received) != n:
            raise ValueError(f"Codeword length must be {n} for r = {r}")

        # Step 4: Decode using correlation
        max_corr = float('-inf')
        decoded_index = -1

        for i, row in enumerate(H):
            dot = sum([a * b for a, b in zip(received, row)])
            if dot > max_corr:
                max_corr = dot
                decoded_index = i

        # Convert decoded index to binary message
        decoded_bits = format(decoded_index, f'0{r}b')

        result_label.config(
        text=f"✅ Decoded Index: {decoded_index}\
        ???? Message Bits: {decoded_bits}"
        )

    except Exception as e:
        messagebox.showerror("Error", f"Invalid input: {e}")

# === Step 5: GUI Setup ===
root = tk.Tk()
root.title("Hadamard Decoder")
root.geometry("450x450")


# Input for r
tk.Label(root, text="Enter value of r (e.g. 4):").pack()
r_entry = tk.Entry(root)
r_entry.pack()


# Input for corrupted codeword
tk.Label(root, text="Enter corrupted codeword (comma-separated 1s and -1s):").pack()
codeword_entry = tk.Entry(root, width=70)
codeword_entry.pack()


# Output label
result_label = tk.Label(root, text="Decoded message will appear here", fg="yellow", font=("Helvetica", 12))
result_label.pack(pady=15)


# Decode button
tk.Button(root, text="Decode", command=decode, bg="yellow").pack()


# Run GUI
root.mainloop() # Corresponds to Step 5 of the original post.
