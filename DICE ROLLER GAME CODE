
import tkinter as tk
from tkinter import messagebox
from PIL import Image, ImageTk, ImageDraw
import random

# Main program class
class DiceGame:
    def __init__(self, window):
        self.window = window
        self.window.title("Dice Roller Game")
        self.window.geometry("900x700")
        self.window.resizable(False, False)
        self.window.config(bg="#1a472a")
        
        self.rolling = False  # to check if dice are rolling
        self.dice_pics = {}  # store dice images here
        
        # make the dice images first
        self.make_dice()
        
        # setup the window
        self.setup_window()
        
        # show welcome popup
        messagebox.showinfo("Welcome!", "Welcome to Dice Roller!\n\nHow to play:\n- Choose how many dice (1 to 6)\n- Press ROLL button\n- See the total!")
    
    def make_dice(self):
        # creating dice faces with dots
        size = 130
        
        # where to put dots for each number
        dots = {
            1: [(65, 65)],
            2: [(40, 40), (90, 90)],
            3: [(40, 40), (65, 65), (90, 90)],
            4: [(40, 40), (90, 40), (40, 90), (90, 90)],
            5: [(40, 40), (90, 40), (65, 65), (40, 90), (90, 90)],
            6: [(40, 35), (90, 35), (40, 65), (90, 65), (40, 95), (90, 95)]
        }
        
        # make each dice (1 to 6)
        for num in range(1, 7):
            img = Image.new('RGB', (size, size), 'white')
            draw = ImageDraw.Draw(img)
            
            # draw the border
            draw.rounded_rectangle([(4, 4), (size-4, size-4)], radius=18, outline='#2b2b2b', width=5)
            
            # draw the dots
            for x, y in dots[num]:
                draw.ellipse([x-12, y-12, x+12, y+12], fill='#1a472a')
            
            self.dice_pics[num] = ImageTk.PhotoImage(img)
    
    def setup_window(self):
        # Title at top
        title = tk.Label(self.window, text="🎲 DICE ROLLER 🎲", font=("Arial", 40, "bold"), 
                        bg="#1a472a", fg="#FFD700")
        title.pack(pady=20)
        
        subtitle = tk.Label(self.window, text="Python Project", font=("Arial", 12), 
                           bg="#1a472a", fg="white")
        subtitle.pack()
        
        # Number of dice selector
        control = tk.Frame(self.window, bg="#1a472a")
        control.pack(pady=20)
        
        tk.Label(control, text="Number of Dice:", font=("Arial", 15, "bold"), 
                bg="#1a472a", fg="white").pack(side="left", padx=10)
        
        self.num_dice = tk.StringVar(value="2")
        selector = tk.Spinbox(control, from_=1, to=6, width=8, font=("Arial", 16, "bold"),
                             textvariable=self.num_dice, state="readonly", justify="center")
        selector.pack(side="left")
        
        # Area where dice will show
        dice_area = tk.Frame(self.window, bg="#2d5a3d", relief="groove", bd=5)
        dice_area.pack(pady=25, padx=50, fill="both", expand=True)
        
        self.message = tk.Label(dice_area, text="Press ROLL to start!", 
                               font=("Arial", 16, "bold"), bg="#2d5a3d", fg="white")
        self.message.pack(pady=50)
        
        self.dice_area = tk.Frame(dice_area, bg="#2d5a3d")
        self.dice_area.pack(pady=30)
        
        # Total score display
        score_box = tk.Frame(self.window, bg="#FFD700", relief="raised", bd=5)
        score_box.pack(pady=20, padx=50, fill="x")
        
        self.score = tk.Label(score_box, text="TOTAL: 0", font=("Arial", 30, "bold"),
                             bg="#FFD700", fg="#1a472a", pady=15)
        self.score.pack()
        
        # Roll button
        self.roll_button = tk.Button(self.window, text="🎲 ROLL DICE 🎲", 
                                     font=("Arial", 20, "bold"), bg="#FF4500", fg="white",
                                     relief="raised", bd=7, cursor="hand2", 
                                     command=self.start_rolling, height=2)
        self.roll_button.pack(pady=20, padx=50, fill="x")
        
        # button effects
        self.roll_button.bind("<Enter>", lambda e: self.roll_button.config(bg="#FF6347"))
        self.roll_button.bind("<Leave>", lambda e: self.roll_button.config(bg="#FF4500"))
    
    def start_rolling(self):
        if self.rolling:
            return  # dont roll if already rolling
        
        self.rolling = True
        self.roll_button.config(state="disabled")
        self.message.pack_forget()
        
        self.do_animation(0)
    
    def do_animation(self, count):
        # animate the rolling effect
        if count < 25:
            # clear old dice
            for widget in self.dice_area.winfo_children():
                widget.destroy()
            
            # show random dice
            how_many = int(self.num_dice.get())
            
            for i in range(how_many):
                random_num = random.randint(1, 6)
                dice = tk.Label(self.dice_area, image=self.dice_pics[random_num], bg="#2d5a3d")
                dice.pack(side="left", padx=15)
            
            # wait time (gets slower)
            wait = 40 + (count * 12)
            self.window.after(wait, lambda: self.do_animation(count + 1))
        else:
            self.show_result()
    
    def show_result(self):
        # clear animation
        for widget in self.dice_area.winfo_children():
            widget.destroy()
        
        how_many = int(self.num_dice.get())
        total = 0
        
        # roll final dice
        for i in range(how_many):
            number = random.randint(1, 6)
            total = total + number
            
            # make a frame for each dice
            frame = tk.Frame(self.dice_area, bg="#1a472a", relief="raised", bd=4)
            frame.pack(side="left", padx=18)
            
            dice = tk.Label(frame, image=self.dice_pics[number], bg="white")
            dice.pack(padx=4, pady=4)
        
        # show total with animation
        self.count_total(0, total)
        
        # enable button again
        self.rolling = False
        self.roll_button.config(state="normal")
    
    def count_total(self, current, final):
        # counting animation for total
        if current < final:
            current = current + 1
            self.score.config(text=f"TOTAL: {current}")
            self.window.after(35, lambda: self.count_total(current, final))
        else:
            self.score.config(text=f"TOTAL: {final}")


# Start the program
def main():
    root = tk.Tk()
    game = DiceGame(root)
    root.mainloop()

if __name__ == "__main__":
    main()
