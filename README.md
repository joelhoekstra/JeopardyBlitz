# Jeopardy Blitz

A spin-off of the monthly game: **a brand-new 3 × 5 board every day**, a hidden Daily Double, and a score that **resets at midnight**. The highest score each day wins that day, and the leaderboard ranks players by days won, then all-time score.

## How it plays

- **New board daily.** 3 categories with 5 clues each ($200 to $1,000). Play the tiles in any order. There are no unlock rules.
- **Score resets daily.** Everyone starts each day at $0. Correct answers add the clue's value, wrong answers subtract it, and a pass costs nothing.
- **Hidden Daily Double.** One tile (never in the $200 row) hides a Daily Double. Wager from $5 up to your score, or $1,000 if your score is lower. You can't pass, and a wrong answer or running out of time loses the whole wager.
- **Timer.** 60 seconds per clue, Daily Double included. Running out of time on a normal clue counts as a pass; on the Daily Double it loses the wager.
- **No take-backs.** Once a clue is revealed it is used. Leaving or reloading mid-clue counts as a pass (or a lost wager on the Daily Double).
- **No appeals,** and answer checking is stricter than the monthly game (a stray letter no longer counts).
- **Winning the day.** The top score wins; ties share the win. Today's leader is credited once the day is over.
- **Leaderboard.** Days won, all-time score, today's score, plus the last 7 finished days.

## Setting it up (copy of the monthly game)

1. **Copy the Google Sheet.** In the copy, delete the `Boards`, `Players`, `GameData` tabs if they came along. Keep `QuestionBank`. The game creates `DailyBoards`, `Results`, and `Players` itself.
2. **New Apps Script project.** Paste in `Code.gs` and add an HTML file named `Index` with the contents of `Index.html`. Set `SPREADSHEET_ID` at the top of `Code.gs` to the copied sheet's ID.
3. **Set the time zone.** Project Settings → Time zone. This decides when "a new day" starts.
4. **Deploy** as a web app (run as yourself, access for whoever will play) and copy the `/exec` link.
5. **Optional: pre-build tomorrow's board.** Run `setupDailyTrigger` once from the Apps Script editor (it will ask for permission). Without it, the first player each day builds the board when they open the game, which works fine but takes a few seconds.
6. **GitHub Pages (optional).** In a new repo, upload the files from `github-pages/`, paste your `/exec` link into `GAME_URL` in `index.html`, and turn on Pages. This gives the home-screen icon (a red version, so it's easy to tell apart from the monthly game).

## Settings

| Setting | Where |
|---|---|
| Clue timer, Daily Double timer | `CLUE_SECONDS`, `DD_SECONDS` in `Index.html` (also the numbers in the rules text) |
| Daily Double wager limits | `DD_MIN_WAGER`, `DD_FLOOR` in `Index.html` |
| Number of Daily Doubles | `DAILY_DOUBLES` in `Code.gs` |
| Board size | `COLS` and `ROWS` in `Code.gs` and `Index.html` (the grid layout also needs a CSS change) |
| Admin PIN | `ADMIN_PIN` in `Index.html` |
| Spreadsheet | `SPREADSHEET_ID` in `Code.gs` |

## Notes and limits

- **Questions.** The game uses about 15 clues a day, so a year needs roughly 5,500. It avoids clues already used on earlier days and falls back to repeats if the QuestionBank runs short. A bank of 200,000 clues lasts decades.
- **Big question banks.** With more than 5,000 rows, the game doesn't read the whole sheet. It jumps to random spots and reads only the rows around each one, so building a board takes a few seconds even with 200,000 rows. This assumes each category's clues sit in neighbouring rows, as they do in the dataset. Keep each category's rows together if you re-sort or add to the bank. Smaller banks are read in full.
- **Today only.** Only today's board can be played or changed. Past days are final.
- **Deleting a player** removes them from the leaderboard and from the days-won counts. Their rows stay in the `Results` tab.
- **Honor system.** The answers are sent to the page, so a determined player could find them. The Admin PIN is also only a light lock.
- **Remembering who's playing.** The first time on a device a player picks their name; after that the game remembers it. A personal link like `https://<your-pages-address>/?player=Sam` opens as that player.
