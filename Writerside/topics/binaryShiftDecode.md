# binaryShiftDecode

Tool to help read binary but if the data order is moved around. This was a tool I made in preparation for Mile High.

Takes in a file with binary a data on each line
ie
```
0011
1011
0110
```

and will try to hex decode

```bash
python main.py
```

a gui will pop up where you can pick a file

### Code {collapsible="true"}
```python
import tkinter as tk
from tkinter import filedialog, messagebox

# Shift a binary string to the right
def shift_binary(binary):
    return binary[-1] + binary[:-1]

# Decode binary string to ASCII character
def decode_binary(binary):
    try:
        return chr(int(binary, 2))
    except ValueError:
        return "?"

# Load binary data from a file
def load_binary_data():
    """Prompt the user to select a file and load binary data."""
    global binary_data, current_data

    # Open file dialog to select a file
    file_path = filedialog.askopenfilename(
        title="Select a file",
        filetypes=(("Text Files", "*.txt"), ("All Files", "*.*"))
    )
    
    if not file_path:
        return  # User canceled the file selection

    try:
        with open(file_path, "r") as file:
            binary_data = [line.strip() for line in file.readlines() if line.strip()]
        
        # Validate binary strings
        for binary in binary_data:
            if len(binary) != 8 or not set(binary).issubset({'0', '1'}):
                raise ValueError(f"Invalid binary string: {binary}")
        
        # Set initial data and update the display
        current_data = binary_data[:]
        update_display()
    except Exception as e:
        messagebox.showerror("Error", f"Failed to load data: {e}")
        binary_data = []
        current_data = []

# Update the display
def update_display():
    """Update the binary strings and their decoded characters displayed in the GUI."""
    for i in range(len(binary_labels)):
        if i < len(current_data):
            binary = current_data[i]
            binary_labels[i].config(text=binary)
            decoded_labels[i].config(text=decode_binary(binary))
        else:
            binary_labels[i].config(text="")  # Clear unused labels
            decoded_labels[i].config(text="")

# Shift binary strings to the right and update the display
def next_page():
    """Shift all binary strings to the right."""
    global current_data
    current_data = [shift_binary(binary) for binary in current_data]
    update_display()

# Initialize the GUI
root = tk.Tk()
root.title("Binary Viewer with Shifts and Decoding")

# Initialize variables
binary_data = []
current_data = []

# Frame for binary strings and decoded characters
frame = tk.Frame(root)
frame.pack(pady=10)

# Create labels for binary strings and decoded characters
binary_labels = []
decoded_labels = []
rows_to_display = 10  # Adjust based on how many rows you want visible at once

for _ in range(rows_to_display):
    row = tk.Frame(frame)
    row.pack(side=tk.TOP, fill=tk.X, pady=2)
    
    binary_label = tk.Label(row, width=15, text="", anchor="w")
    binary_label.pack(side=tk.LEFT, padx=5)
    binary_labels.append(binary_label)
    
    decoded_label = tk.Label(row, width=5, text="", anchor="w")
    decoded_label.pack(side=tk.LEFT, padx=5)
    decoded_labels.append(decoded_label)

# Buttons for file loading and shifting
button_frame = tk.Frame(root)
button_frame.pack(pady=5)

load_button = tk.Button(button_frame, text="Load File", command=load_binary_data)
load_button.pack(side=tk.LEFT, padx=5)

next_button = tk.Button(button_frame, text="Next", command=next_page)
next_button.pack(side=tk.LEFT, padx=5)

# Start the GUI event loop
root.mainloop()


```