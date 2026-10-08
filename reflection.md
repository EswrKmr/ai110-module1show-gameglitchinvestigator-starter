# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

  When I ran the game for the first time I tried to enter my inputs using my enter button on the keyboard. I realized that it didnt work so I had to manually click "Submit" for it to work. I later started messing around with numbers and it didnt not tell me to stay between 1-100 if I entered a negative number or a number over 100. It will tell me to go higher or lower for both of those cases, so I assumed that the higher and lower code is randomized when a number is outside of the 1-100 bounds. 

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
|   1   | Go Higher!            Go Lower!       none /app,py,checkguess
|  3456 | Enter a number      Go Higher/Lower!  none /app,py,checkguess
          between 1-100     

|New game| Starts a new game.  Doesnt work.     none / app.py, newgame

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.

I asked Claude to fix the bug where guesses over 100 were
accepted. It suggested adding a range check right after `parse_guess` in `app.py`,
using the difficulty's `low` and `high` values and reusing the existing error path.
This was correct because it fixes the bug in one place without changing how errors
are displayed, and it also respects Easy (1-20) and Hard (1-50), not just Normal.
I verified it by guessing 150, 0, and -5 and checking that an error appeared
instead of a hint.

Claude's st.form fix moved the New Game button and hint checkbox out of the form
because Streamlit only allows form_submit_button inside a form. I accepted the
idea but checked that the layout still worked, because it changed the original
three-column layout. Claude noticed other problems (like the string comparison on even attempts) but
I told it to stay in scope and only fix the bugs I listed.

---

## 3. Debugging and testing your fixes

Entered 150, 0, -5 on Normal; each showed "Please enter a number between 1 and 100." Switched to Easy and entered 21; same error. Typed a number and pressed Enter without clicking the button; the guess was submitted. Ran `python3 -m streamlit run app.py` after moving the functions to logic_utils.py; the app loaded with no import errors. Guessed above
the secret (shown in Developer Debug Info) and got "Go LOWER!". Note: hints on even-numbered attempts may still look wrong. That comes from `app.py` converting the secret to a string on even attempts, a separate bug I
did not fix in this round.

---

## 4. What did you learn about Streamlit and state?


Every time you click a button, type in a box, or change a setting, Streamlit
re-runs the entire Python script from top to bottom. That means normal variables
get reset on every run, like a whiteboard that gets erased each time. Session
state (`st.session_state`) is the part that doesn't get erased. It's a dictionary
that remembers values between reruns, so things like the secret number, the score,
and the attempt count stay put. In this game, the pattern `if "secret" not in
st.session_state:` means "only set this the first time," so the secret doesn't
change on every guess. I also learned that a button only returns True on the one
rerun right after it's clicked, and that putting the text box and submit button
inside `st.form` is what makes the Enter key submit the guess.

---

## 5. Looking ahead: your developer habits

**One habit or strategy I want to reuse:**
Marking the exact spot of a bug with a `# FIXME` comment and fixing one bug at a
time. It kept me and the AI focused on a single problem, and it made it easy to
check each change afterward. 

**One thing I would do differently next time with AI on a coding task:**
Write a few small tests (for example, for `check_guess` and `parse_guess`) before
asking the AI to change the code, so I can prove a fix works instead of only
checking it by hand in the game. 

**How this project changed the way I think about AI-generated code:**
AI-generated code can look clean and still be full of bugs, so I can't assume it's
correct just because it runs. I need to read it, test it, and stay in control of
what gets accepted. 
