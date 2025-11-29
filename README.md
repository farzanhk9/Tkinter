import tkinter as tk
from tkinter import filedialog, messagebox

class TextEditor:
    def __init__(self, root):
        self.root = root
        self.root.title("Simple Text Editor")
        self.root.geometry("600x400")

        # Text widget for writing and editing
        self.text_area = tk.Text(root, wrap="word", font=("Arial", 12))
        self.text_area.pack(expand=True, fill="both")

        # Menu bar
        self.menu_bar = tk.Menu(root)
        self.root.config(menu=self.menu_bar)

        # File menu
        self.file_menu = tk.Menu(self.menu_bar, tearoff=0)
        self.menu_bar.add_cascade(label="File", menu=self.file_menu)
        self.file_menu.add_command(label="New", command=self.new_file)
        self.file_menu.add_command(label="Open", command=self.open_file)
        self.file_menu.add_command(label="Save", command=self.save_file)
        self.file_menu.add_command(label="Exit", command=root.quit)

        # Edit menu
        self.edit_menu = tk.Menu(self.menu_bar, tearoff=0)
        self.menu_bar.add_cascade(label="Edit", menu=self.edit_menu)
        self.edit_menu.add_command(label="Undo", command=self.undo)
        self.edit_menu.add_command(label="Redo", command=self.redo)

    def new_file(self):
        """Clear the text area to create a new file."""
        if self.confirm_unsaved_changes():
            self.text_area.delete(1.0, tk.END)

    def open_file(self):
        """Open a file and display its content."""
        if self.confirm_unsaved_changes():
            file_path = filedialog.askopenfilename(defaultextension=".txt", filetypes=[("Text files", "*.txt"), ("All files", "*.*")])
            if file_path:
                with open(file_path, "r") as file:
                    content = file.read()
                self.text_area.delete(1.0, tk.END)
                self.text_area.insert(tk.END, content)

    def save_file(self):
        """Save the current content to a file."""
        file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Text files", "*.txt"), ("All files", "*.*")])
        if file_path:
            with open(file_path, "w") as file:
                file.write(self.text_area.get(1.0, tk.END))

    def undo(self):
        """Undo the last action."""
        self.text_area.edit_undo()

    def redo(self):
        """Redo the last undone action."""
        self.text_area.edit_redo()

    def confirm_unsaved_changes(self):
        """Check if there are unsaved changes and confirm with the user."""
        if self.text_area.get(1.0, tk.END) != "\n":
            response = messagebox.askyesno("Unsaved changes", "You have unsaved changes. Do you want to continue without saving?")
            return response
        return True

if __name__ == "__main__":
    root = tk.Tk()
    editor = TextEditor(root)
    root.mainloop()
