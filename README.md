 2311CS030550-Simple-text-editor
Created a simple text editor in Python using arrays, stacks, and queues. It supports text insertion, deletion, undo/redo, find and replace, and a print queue, with file handling for saving and loading data.
import os

class TextEditor:
    def __init__(self):
        self.filename = "text_editor_file.txt"
        self.text = []
        self.undo_stack = []
        self.redo_stack = []
        self.print_queue = []
        self.load_file()

    def load_file(self):
        if os.path.exists(self.filename):
            with open(self.filename, 'r') as file:
                self.text = file.readlines()

    def save_file(self):
        with open(self.filename, 'w') as file:
            file.writelines(self.text)

    def insert_text(self, text):
        self.text.append(text)
        self.undo_stack.append(text)

    def delete_text(self):
        if self.text:
            deleted_text = self.text.pop()
            self.undo_stack.append(deleted_text)

    def undo(self):
        if self.undo_stack:
            undone_text = self.undo_stack.pop()
            self.redo_stack.append(undone_text)
            self.text.append(undone_text)

    def redo(self):
        if self.redo_stack:
            redone_text = self.redo_stack.pop()
            self.undo_stack.append(redone_text)
            self.text.append(redone_text)

    def find_text(self, text):
        for i, line in enumerate(self.text):
            if text in line:
                return i
        return -1

    def replace_text(self, old_text, new_text):
        for i, line in enumerate(self.text):
            if old_text in line:
                self.text[i] = line.replace(old_text, new_text)

    def print_text(self):
        print("\n".join(self.text))

    def add_print_job(self, text):
        self.print_queue.append(text)

    def delete_print_job(self):
        if self.print_queue:
            self.print_queue.pop()

def main():
    editor = TextEditor()

    while True:
        print("\nText Editor Menu:")
        print("1. Insert Text")
        print("2. Delete Text")
        print("3. Undo")
        print("4. Redo")
        print("5. Find Text")
        print("6. Replace Text")
        print("7. Print Text")
        print("8. Add Print Job")
        print("9. Delete Print Job")
        print("10. Save File")
        print("11. Quit")

        choice = input("Enter your choice: ")

        if choice == "1":
            text = input("Enter text to insert: ")
            editor.insert_text(text + "\n")
        elif choice == "2":
            editor.delete_text()
        elif choice == "3":
            editor.undo()
        elif choice == "4":
            editor.redo()
        elif choice == "5":
            text = input("Enter text to find: ")
            result = editor.find_text(text)
            if result != -1:
                print(f"Text found at line {result + 1}")
            else:
                print("Text not found")
        elif choice == "6":
            old_text = input("Enter text to replace: ")
            new_text = input("Enter new text: ")
            editor.replace_text(old_text, new_text)
        elif choice == "7":
            editor.print_text()
        elif choice == "8":
            text = input("Enter text to add to print queue: ")
            editor.add_print_job(text)
        elif choice == "9":
            editor.delete_print_job()
        elif choice == "10":
            editor.save_file()
            print("File saved successfully!")
        elif choice == "11":
            editor.save_file()
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
