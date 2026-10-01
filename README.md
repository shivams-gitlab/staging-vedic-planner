v92


Implemented, with one thing worth flagging clearly: your earlier design decision was "no user data stored, anywhere, period." This feature can't work without one small, explicit exception to that — Google's silent-login API needs a hint (the account's email address) to know which of several signed-in Chrome accounts to try. So:

What's now stored: just the email address of the account you last connected with, in localStorage, on your own device. Never the access token, never sent to any server.
How it behaves:
Page load (silent reconnect): uses the saved hint, so Google tries that specific account directly — no chooser popup, assuming that account's Google session is still active in the browser.
Clicking "Connect Calendar" (interactive): never sends a hint, so it always shows the full account chooser, letting you switch accounts freely. Whichever account you pick there becomes the new saved hint for future silent reconnects.
Disconnect: clears the saved hint entirely, so it won't try to silently reconnect to that account next time.
Also added the email OAuth scope (needed to ask Google "whose token is this") — it's classified as non-sensitive, so it doesn't add to the verification burden on top of calendar.events.
