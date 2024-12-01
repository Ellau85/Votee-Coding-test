# Votee-Coding-test
import random
import requests
import datetime

# Replace wit your OpenAI API Key
API_KEY = 'your_openai_api_key'

# List of possible words to guess
word_list = ['apple', 'banana', 'cherry', 'date', 'elderberry', 'fig', 'grape'] 

# Functions to get the daily word
def get_daily_word ():
# Seed random number generator for reproducibility
random.seed(datetime.date.today())
return random.choice(word_list)

# Funtion to send a guess to OpenAI API for validation
def guess_word(guess, daily_word):
prompt = f"Is the word '{guess}' the correct word? The word to guess is '{daily_word}'."
response = requests.post (
'https://api.openai.com/v1/chat/completions',
headers={'Authorization': f'Bearer {API_KEY}',
'Content-Type': 'application/json'
},
json={
'model': 'gpt-3.5-turbo',
'messages': [{'role': 'user', 'content': prompt}]
}
)

return response.json()

# Main function
def main():
daily_word = get_daily_word()
print("Welcome to the Daily Word Guessing Game!")
print("Try to guess the word of the day!")

while True:
guess = input("Enter your guess: ").strip().lower()
response = guess_word(guess, daily_word)

if 'choices' in response and response['choices']:
answer = response['choices'][0]['message']['content']
print(answer)

if guess == daily_word:
print("Congratulations! You 've guessed the word!")
break
else:
print("Sorry, there was an error with the API response.")

if __name__ == '__main__":
main()
