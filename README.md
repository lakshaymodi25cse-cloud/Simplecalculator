import tkinter as tk

class ModernCalculator:
    def __init__(self, root):
        self.root = root
        self.root.title("Python Calculator")
        self.root.geometry("320x450")
        self.root.configure(bg="#17161b")
        self.root.resizable(False, False) # Prevents resizing to keep UI clean

        self.equation = ""
        self.display_var = tk.StringVar()

        self._build_ui()

    def _build_ui(self):
        # --- Display Screen ---
        display = tk.Entry(
            self.root, 
            textvariable=self.display_var, 
            font=('Helvetica', 28, 'bold'),
            bg="#17161b", 
            fg="#ffffff", 
            bd=0, 
            justify="right"
        )
        display.pack(fill="both", ipadx=8, ipady=20, pady=10)

        # --- Button Frame ---
        btn_frame = tk.Frame(self.root, bg="#17161b")
        btn_frame.pack()

        # Button Styling Parameters
        btn_font = ('Helvetica', 16, 'bold')
        btn_width = 5
        btn_height = 2

        # Grid Layout Data (Text, Row, Column, Background Color, Text Color)
        buttons = [
            ('C', 0, 0, "#3697f5", "#ffffff"), 
            ('(', 0, 1, "#2a2d36", "#ffffff"), 
            (')', 0, 2, "#2a2d36", "#ffffff"), 
            ('/', 0, 3, "#2a2d36", "#ffffff"),

            ('7', 1, 0, "#2a2d36", "#ffffff"), 
            ('8', 1, 1, "#2a2d36", "#ffffff"), 
            ('9', 1, 2, "#2a2d36", "#ffffff"), 
            ('*', 1, 3, "#2a2d36", "#ffffff"),

            ('4', 2, 0, "#2a2d36", "#ffffff"), 
            ('5', 2, 1, "#2a2d36", "#ffffff"), 
            ('6', 2, 2, "#2a2d36", "#ffffff"), 
            ('-', 2, 3, "#2a2d36", "#ffffff"),

            ('1', 3, 0, "#2a2d36", "#ffffff"), 
            ('2', 3, 1, "#2a2d36", "#ffffff"), 
            ('3', 3, 2, "#2a2d36", "#ffffff"), 
            ('+', 3, 3, "#2a2d36", "#ffffff"),

            ('0', 4, 0, "#2a2d36", "#ffffff"), 
            ('.', 4, 1, "#2a2d36", "#ffffff"), 
            ('00', 4, 2, "#2a2d36", "#ffffff"), 
            ('=', 4, 3, "#fa4837", "#ffffff")
        ]

        # Generate Buttons dynamically
        for (text, row, col, bg_color, fg_color) in buttons:
            btn = tk.Button(
                btn_frame, text=text, font=btn_font, bg=bg_color, fg=fg_color,
                width=btn_width, height=btn_height, bd=0, cursor="hand2",
                activebackground="#424651", activeforeground="#ffffff",
                command=lambda t=text: self._on_button_click(t)
            )
            btn.grid(row=row, column=col, padx=2, pady=2)

    def _on_button_click(self, char):
        if char == 'C':
            self.equation = ""
            self.display_var.set(self.equation)
        elif char == '=':
            try:
                # Evaluate the math expression safely
                result = str(eval(self.equation))
                self.display_var.set(result)
                self.equation = result # Allow chaining operations after hitting '='
            except ZeroDivisionError:
                self.display_var.set("Error: Div by 0")
                self.equation = ""
            except Exception:
                self.display_var.set("Error")
                self.equation = ""
        else:
            self.equation += str(char)
            self.display_var.set(self.equation)


if __name__ == "__main__":
    app = tk.Tk()
    calculator = ModernCalculator(app)
    app.mainloop()
