# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

## Document Your Experience

**Bugs fixed:**
Guesses outside the difficulty's range (like 150) were accepted. Added a range check in `app.py`.
Pressing Enter did not submit. Wrapped the input in `st.form`.
Hints were backwards ("Too High" said Go HIGHER). Swapped the messages in `check_guess`.

Moved `get_range_for_difficulty`, `parse_guess`, `check_guess`, and `update_score`
from `app.py` into `logic_utils.py` and imported them back into `app.py`.

**How I used AI:** I used Claude in chat (not agent mode) to explain the cause of a glitch,
write the fixes, and generate the refactor. I reviewed each change before using it.

**Known issues I did not fix:**
- On even-numbered attempts `app.py` converts the secret to a string, so hints can be
  wrong for those guesses.
- The attempt counter starts at 1, so the game allows one fewer guess than the sidebar says.
- A rejected out-of-range guess still uses up an attempt.

## 📸 Demo Walkthrough

## Demo Walkthrough

Sample game on Normal difficulty (range 1 to 100), secret number = 50:

1. User enters 40 and presses Enter. The game shows "📈 Go HIGHER!" and the score goes to -5.
2. User enters 70. The game shows "📉 Go LOWER!" and the score goes to -10.
3. User enters 50. The game shows balloons and "You won! The secret was 50. Final score: 40."
4. Further guesses are blocked until the user clicks New Game.

Input checks:
- Entering 150, 0, or -5 shows "Please enter a number between 1 and 100." instead of a hint.
- Entering text like "abc" shows "That is not a number."
- Pressing Enter in the text box submits the guess, with no need to click the button.

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```
# Paste your pytest output here, e.g.:
# pytest tests/
# ========================= X passed in 0.XXs =========================
```

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]
