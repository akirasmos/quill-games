# The Quill Games

A daily games section for *The Quill*, the student news publication of Morris
Knolls High School. It's styled to match mkquill.com and is meant to sit inside
a page on that site, under The Quill's own masthead and menu:

- **Quirdle**: guess the hidden 5-letter word in 6 tries.
- **Quini**: a 5×5 daily crossword with a timer.

It's plain HTML, CSS and JavaScript with no build step and no server code, so it
works on any host: GitHub Pages, Netlify, your school's web server, or an
"embed HTML" block in WordPress, Squarespace or Wix (upload the folder and link to it).

```
quill-games/
├── index.html              Games home page (links to both games)
├── quirdle.html            Quirdle
├── quini.html              Quini crossword
├── mini.html               Redirects old links to quini.html
├── EDITORS.md              How to make puzzles, permissions, going live
├── editor/
│   ├── quirdle.html        Schedule Quirdle words by date
│   └── quini.html          Build, check and test-solve a Quini
├── css/games.css           Shared styles (colors are at the top)
└── js/
    ├── quirdle-answers.js  Quirdle words by date  (Quirdle editors only)
    ├── quini-puzzles.js    Quini puzzles          (Quini editor only)
    ├── quirdle-valid.js    Words players may guess (no need to edit)
    ├── common.js           Launch date, saving, pop-ups, keyboard
    ├── quirdle.js
    └── quini.js
```

**Making puzzles, controlling who can publish them, and putting the games on
mkquill.com are all covered in [EDITORS.md](EDITORS.md).**

## Try it locally

```sh
cd quill-games
python3 -m http.server 8000
# open http://localhost:8000
```

(Opening `index.html` by double-clicking works too.)

## Daily puzzles

Every player gets the same puzzle on the same day. The puzzle changes at local
midnight. Day 1 is `LAUNCH_DATE` at the top of `js/common.js`. Set it to the day
you go live.

- **Quirdle:** a word scheduled for today's date in `js/quirdle-answers.js`
  wins. Otherwise the next word from the backup list is used.
- **Quini:** a puzzle whose `date` is today wins. Otherwise the undated
  puzzles take turns, so keep adding them.

Use the editor pages to change either one. They check everything and produce
the finished file text. See [EDITORS.md](EDITORS.md).

## Customizing the look

The games have no header, nav or footer of their own. Those come from the
mkquill.com page they're embedded in (see [EDITORS.md](EDITORS.md)).

The colors are CSS variables at the top of `css/games.css`: `--brand` is the
site's green (`#127c00`), `--band` is the gray behind section headings, and
`--gold` matches the feather in the logo. The games always display in light
mode, like mkquill.com. Fonts are Alegreya (headings) and Lato (text) from
Google Fonts.

## Notes

- Progress, stats and streaks are saved in each player's browser (`localStorage`).
  There are no accounts and nothing is sent to a server.
- Since everything runs in the browser, a curious student could find the answers
  in the page source. That's normal for games like this.
