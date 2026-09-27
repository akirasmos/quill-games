# Running The Quill Games: a guide for editors

This covers three things: who is allowed to publish puzzles, how to make a
Quirdle or a Quini, and how to hook it all up to mkquill.com.

## How permissions work

The games are plain files on a website, so "who can make puzzles" really means
**who can change two files**:

| File | What it controls | Who can change it |
|---|---|---|
| `js/quirdle-answers.js` | Which Quirdle word runs on which day | The Quirdle editors (a few people) |
| `js/quini-puzzles.js` | Every Quini puzzle | The Quini editor (one person) |

Those rules live in **`.github/CODEOWNERS`** and are enforced by GitHub.
Anyone else can *suggest* a change, but it can't go live until the right person
approves it.

The two editor pages (`editor/quirdle.html` and `editor/quini.html`) are safe to
leave online. Anyone can open them and draft puzzles, but they can't publish
anything. Publishing only happens through GitHub.

> Heads-up: everything a browser plays has to be sent to the browser, so a
> determined student could dig answers out of the page source. Permissions
> control who can **change** puzzles, not who can peek at them.

## One-time setup on GitHub

Do this once, as the owner of the repository that hosts the games.

1. **The games live in their own repository**, `akirasmos/quill-games`, with
   `.github/CODEOWNERS` at the top level. Nothing to do here.
2. **Add your editors as collaborators.** Repo **Settings → Collaborators →
   Add people**. Give them the **Write** role. Each person needs a free GitHub
   account.
3. **Name who owns what.** Edit `.github/CODEOWNERS` and replace `@akirasmos`
   with real usernames:
   ```
   /js/quirdle-answers.js   @quirdle-editor-1 @quirdle-editor-2 @quirdle-editor-3
   /js/quini-puzzles.js     @quini-editor
   ```
   Keep yourself as the owner of `/.github/CODEOWNERS` so nobody can change
   the rules.
4. **Turn on enforcement.** Repo **Settings → Branches → Add branch
   protection rule** (or **Settings → Rules → Rulesets → New branch ruleset**)
   for the `main` branch, and check:
   - Require a pull request before merging
   - Require approvals (1)
   - Require review from Code Owners

   Without this step, CODEOWNERS only *suggests* reviewers and doesn't block
   anything.

**When the Quini editor publishes their own puzzle:** GitHub never lets people
approve their own changes. So when the Quini editor submits a puzzle, you (the
repo admin) finish it by clicking **Merge without waiting for requirements to
be met** on the pull request. When anyone *else* touches the Quini file, only the
Quini editor's approval can publish it. The Quirdle file works the same way, but
with several owners they can simply approve each other's schedules.

## Making a Quirdle

1. Open `editor/quirdle.html` (on the live site, or by double-clicking the file
   on your computer).
2. Type a word next to any date. Use **Start date** and **Show** (up to a
   year) to reach dates further ahead. Any 5-letter word works, including
   school words that aren't in the backup list, like a mascot or teacher's
   name. Each word is checked right away:
   - it must be 5 letters
   - it can't already be used on another date
   - if it isn't in the dictionary, you'll see a note asking you to check the
     spelling. It still works as the answer, since scheduled words are always
     accepted as guesses.
3. Days you leave blank use the next word from the **backup list** (the gray
   words), so there's never a missing puzzle. You can edit the backup list at
   the bottom of the page too.
4. Click **Download quirdle-answers.js**.
5. On GitHub, open the `js` folder → **Add file → Upload files** → drop in the
   downloaded file (it replaces the old one) → choose **Create a new branch and
   start a pull request** → **Propose changes**.
6. A Quirdle editor approves it and clicks **Merge**. It's live within a
   minute or two.

## Making a Quini

1. Open `editor/quini.html`.
2. Build the grid. Click a square and type. Press `.` or `#` for a black
   square, and press Space to switch between typing across and down. Tip:
   start from a published puzzle with **Load a published puzzle…** and change
   it.
3. Write a clue for every word. The clue boxes appear automatically as words
   form.
4. Watch the **Checks** list until it says **✓ Ready to publish**. It flags
   empty squares, missing clues, repeated words, 5-letter words it doesn't
   recognize, and letters that aren't in both an across and a down word.
5. Click **Test solve** to play it exactly as readers will. Test solves don't
   touch your real stats.
6. Optional: set a **Date** to run it on a specific day. Puzzles without a
   date rotate on all other days.
7. Click **Copy puzzle entry**. On GitHub, open `js/quini-puzzles.js` → the
   pencil (**Edit**) icon → paste the entry at the end of the list, just above
   the final `];` → **Commit changes… → Create a new branch and start a pull
   request**.
8. The Quini editor approves and merges (or you do, if the Quini editor wrote
   it; see above).

Good Quini habits: keep 5-letter words common, avoid obscure abbreviations, and
give school-themed clues when you can. The first puzzles in the file are good
examples.

## Putting it on mkquill.com

The code stays on GitHub, and GitHub Pages turns it into a live website for
free. mkquill.com then gets a **Games** page that shows it. That way every
change you merge on GitHub shows up on the newspaper site automatically, and
the puzzle permissions keep working.

### Step 1: Turn on GitHub Pages

In the `quill-games` repo: **Settings → Pages → Build and deployment →
Source: Deploy from a branch → Branch: `main`, folder `/ (root)` → Save**.

After a minute or two the games are live at
**https://akirasmos.github.io/quill-games/**. Open it and play a round to
check.

### Step 2: Add a Games page on mkquill.com

mkquill.com looks like a WordPress site. You'll need an **Administrator** or
**Editor** account on it.

1. In the WordPress dashboard: **Pages → Add New**. Title it `Games`.
2. Add a **Custom HTML** block (click **+**, search "Custom HTML") and paste:
   ```html
   <iframe src="https://akirasmos.github.io/quill-games/"
           title="The Quill Games"
           style="width:100%;height:950px;border:0;display:block"
           allow="clipboard-write; web-share"></iframe>
   ```
   The `allow` part lets the **Share** buttons copy results.
3. **Publish**. The page will be at something like `mkquill.com/games/`.
4. Add it to the menu: **Appearance → Menus** (or **Appearance → Customize →
   Menus**) → tick **Games** under Pages → **Add to Menu** → drag it where
   you want → **Save Menu**.

The games have no header of their own. On the Games page, readers see The
Quill's normal masthead and menu (the same as every other page on the site),
with the games underneath. Opened directly at the GitHub address, the games
start straight at the gray "Games" heading.

**If the Custom HTML block is missing or your iframe disappears when you
save:** some hosts (including student-newspaper hosting services) only let
Administrators add iframes. Ask your adviser or the site admin to paste it, or
contact the host's support and ask how to embed an iframe. If embedding isn't
allowed at all, add a **Custom Link** to the menu instead
(**Appearance → Menus → Custom Links**, URL
`https://akirasmos.github.io/quill-games/`, text `Games`). Readers won't see
the site's masthead on that page, though, so ask your host about embedding
first.

### Step 3 (optional): A nicer address

To use `games.mkquill.com` instead of the github.io address:

1. In the repo: **Settings → Pages → Custom domain** → `games.mkquill.com` →
   **Save**.
2. Whoever manages the mkquill.com domain (your adviser, school IT, or the
   hosting company) adds a DNS record: type `CNAME`, name `games`, value
   `akirasmos.github.io`.
3. Once it works, tick **Enforce HTTPS** on the Pages settings screen and
   change the iframe's `src` to `https://games.mkquill.com/`.

### Before launch day

- Set `LAUNCH_DATE` in `js/common.js` to the day you go live, so that day is
  puzzle #1.
- Don't add the editor pages to the menu. Editors can bookmark
  `…/quill-games/editor/quirdle.html` and `…/editor/quini.html`.

### What about uploading the files to the newspaper's server?

It's possible if your host gives you file access (SFTP or a file manager):
upload everything into a `games` folder and it's live at
`mkquill.com/games/`. But you'd have to re-upload after every puzzle change,
and GitHub's approval rules no longer control what goes live. Most
student-newspaper hosts don't allow it anyway. The GitHub Pages setup above
is simpler and keeps your permissions working.
