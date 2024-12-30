# CodeAlpha_Task1
import random

# Expanded list of possible words
word_list = [
    "python", "hangman", "programming", "developer", "algorithm",
    "function", "variable", "software", "computer", "hardware",
    "internet", "artificial", "intelligence", "machine", "learning",
    "data", "science", "cloud", "network", "database", "cybersecurity"
]


# Function to select a random word
def get_random_word():
    return random.choice(word_list)


# Function to display the current state of the word
def display_word(word, guessed_letters):
    return ''.join([letter if letter in guessed_letters else '_' for letter in word])


# Function to provide a hint (reveal a random letter from the word)
def give_hint(word, guessed_letters):
    unguessed_letters = [letter for letter in word if letter not in guessed_letters]
    if unguessed_letters:
        hint_letter = random.choice(unguessed_letters)
        print(f"Hint: One of the letters is '{hint_letter}'.")
        return hint_letter
    return None


# Function to handle the Hangman game
def play_hangman():
    # Select a random word
    word = get_random_word()
    word_length = len(word)
    guessed_letters = set()  # Store guessed letters
    incorrect_guesses = 0
    max_incorrect_guesses = 6  # You can adjust this as needed (6 chances is typical)
    hint_given = False

    print("Welcome to Hangman!")
    print(f"The word has {word_length} letters.")

    while incorrect_guesses < max_incorrect_guesses:
        # Display the current state of the word
        current_display = display_word(word, guessed_letters)
        print("\nCurrent word: " + current_display)

        # If no hint has been given, provide one after 3 incorrect guesses
        if not hint_given and incorrect_guesses >= 3:
            give_hint(word, guessed_letters)
            hint_given = True

        # Ask the player for a letter guess
        guess = input("Guess a letter: ").lower()

        # Check if the input is a valid single letter
        if len(guess) != 1 or not guess.isalpha():
            print("Please enter a valid single letter.")
            continue

        # If the letter was already guessed, prompt the player again
        if guess in guessed_letters:
            print(f"You've already guessed '{guess}'. Try again.")
            continue

        # Add the guessed letter to the guessed letters set
        guessed_letters.add(guess)

        # Check if the guessed letter is in the word
        if guess in word:
            print(f"Good job! '{guess}' is in the word.")
        else:
            incorrect_guesses += 1
            print(f"Oops! '{guess}' is not in the word.")
            print(f"You have {max_incorrect_guesses - incorrect_guesses} incorrect guesses remaining.")

        # Check if the player has won
        if all(letter in guessed_letters for letter in word):
            print(f"\nCongratulations! You've guessed the word: {word}")
            break

    # If the player runs out of incorrect guesses
    if incorrect_guesses == max_incorrect_guesses:
        print(f"\nGame over! The word was: {word}")


# Run the game
if __name__ == "__main__":
    play_hangman()
