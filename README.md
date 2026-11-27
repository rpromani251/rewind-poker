# Rewind: six-handed hold'em coach

A poker trainer that runs entirely in your browser. You play six-handed no-limit hold'em cash games (blinds 1/2, 200-chip starting stacks) against five simulated opponents. Every couple of hands, a coach rewinds to one decision you faced and asks two or three questions: count the pot, calculate the price, infer a range, choose and defend a play. Missed skills come back later with new numbers or new cards.

It is one file, `index.html`, with no build step and no server code.

## Put it on GitHub Pages

1. Create a new repository on github.com (for example `rewind-poker`).
2. Upload `index.html` (and this README) to the repository's main branch: **Add file → Upload files → Commit changes**.
3. Open **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, then **main** and **/ (root)**, and save.
4. After a minute or so the site is live at `https://<your-username>.github.io/rewind-poker/`.

On a free plan Pages needs a public repository. GitHub Pro, which comes with the Student Developer Pack, can publish from a private one. Either way the published site is public, but your data is not: it stays in your own browser (see below).

To update the app later, upload a new `index.html` over the old one. Your saved data is not affected.

## Run it locally

Double-click `index.html` to open it in Chrome, Edge, Firefox or Safari. Or, from this folder:

```
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Where your data is saved

- **Every hand you play** (full action history, your cards, cards shown at showdown) is saved in the browser's IndexedDB for that site, with no limit on the number of hands.
- **Your progress** (skills, due reviews, the review journal, settings and the current table) is saved in the browser's localStorage.
- The app asks the browser to keep this storage, so it isn't cleared automatically when space runs low.

Data belongs to one browser on one address. `localhost`, your GitHub Pages URL and the claude.ai version each keep separate data. To move or protect it, go to **Progress → Your data**:

- **Download backup** saves everything as a `.json` file.
- **Restore from backup** loads a backup file, replacing what is there.
- **Erase all saved data** starts over.

A weekly backup is a good habit. Clearing site data in your browser settings deletes your saves.

## What it saves about your play (the You tab)

- Results: hands, net chips, bb/100, VPIP, PFR, aggression, fold to bet, went to showdown and won at showdown.
- Results by position.
- A review record per skill, comparing your last 10 reviews with the 10 before.
- Recurring mistakes, for example dividing by the pot before your call, picking impossible hands, choosing actions your own estimate didn't support, and spot types where most of your plays weren't defensible.
- A review journal: every reviewed decision with your answers, the takeaway, and a button to review it again.

## GTO coverage

GTO references exist only for **heads-up river decisions**. The app solves a restricted game itself with CFR+: check or one bet size; facing a bet, call or fold; no raises; no rake. It starts from stated ranges: the opponent's range from the public player-type model, and yours from the solid-regular model of your own actions. Each result shows its positions, effective stack, pot, ranges, iteration count and exploitability, and the **Re-run** button reproduces it exactly.

Every other spot shows "GTO reference unavailable" and still covers the pot math, the range read and the opponent-specific adjustment. Preflop charts are heuristic player-type models, not GTO.

**Benchmark** (Progress tab): pot 100, a 75-chip bet, a bettor with half value hands and half bluff candidates, and a defender holding a pure bluff-catcher. The solver gives: bet 3/7 of bluff candidates, call 4/7 of the time, 30% of bets are bluffs.

## How the opponents work

Each opponent is one of four player types (tight-passive, solid regular, loose-aggressive, calling station) with hidden random adjustments. Each bot acts on its own cards and the public action. The coach never sees bot settings, mucked cards or later cards. It infers ranges from the actions you could see, weighting the four types by how well they explain everything you have observed from that player.

## Differences from the claude.ai version

The claude.ai version saves to your Claude account and can add Claude's written assessment of your reasoning. Here, saving is per browser, and the assessment uses the built-in checklist instead.

Keys: **F** fold · **C** check or call · **R** bet or raise.
