from tkinter import *
import random

# List of possible words
words = ['apple', 'banana', 'mango', 'grape', 'orange']

# Function to select a random word
def get_word():
    return random.choice(words)

# Function to start a new game
def new_game():
    global word, word_length, guessed, attempts, guessed_letters
    word = get_word()
    word_length = len(word)
    guessed = ['_'] * word_length
    attempts = word_length + 2
    guessed_letters = []
    word_label.config(text=' '.join(guessed))
    result_label.config(text="")
    entry.delete(0, END)
    guess_button.config(state=NORMAL)

# Function to handle letter guessing
def guess_letter():
    letter = entry.get().lower()
    if letter in guessed_letters:
        result_label.config(text="You already guessed that letter.")
    elif letter in word:
        for i in range(word_length):
            if word[i] == letter:
                guessed[i] = letter
        word_label.config(text=' '.join(guessed))
        result_label.config(text="Good guess!")
    else:
        global attempts
        attempts -= 1
        result_label.config(text=f"Wrong guess. You have {attempts} attempts left.")
    
    guessed_letters.append(letter)
    entry.delete(0, END)

    if '_' not in guessed:
        result_label.config(text="Congratulations! You guessed the word.")
        guess_button.config(state=DISABLED)
    elif attempts == 0:
        result_label.config(text=f"Sorry, you ran out of attempts. The word was '{word}'.")
        guess_button.config(state=DISABLED)

# Initialize the main window
root = Tk()
root.title("Hangman Game")
root.geometry("400x300")

# Start a new game
new_game()

# UI Elements
Label(root, text="Welcome to Hangman!", font="calibri 20 bold").pack()
word_label = Label(root, text=' '.join(guessed), font="calibri 15")
word_label.pack(pady=10)

entry = Entry(root)
entry.pack(pady=10)

guess_button = Button(root, text="Guess", command=guess_letter)
guess_button.pack(pady=10)

result_label = Label(root, text="", font="calibri 15")
result_label.pack(pady=10)

reset_button = Button(root, text="New Game", command=new_game)
reset_button.pack(pady=10)

# Start the Tkinter loop
root.mainloop()
