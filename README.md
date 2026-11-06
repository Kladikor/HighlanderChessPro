# Highlander Chess Pro v0.7

Beta, recommend sandbox testing only at this time.

## Welcome

Highlander Chess Pro is a new more harder and complicated game with a 10x10 board.
Over 10^200 possible games compared with legacy chess has only 10^120.

Featuring powerful new highlander pieces such as trebuchet and berserker.

Play computer levels from **Wee Bairn** (easy) to **Cromwell** (strongest level in this version).
Future version planned to have a much harder **Shannon** level.

Or invite a friend and play over SMS with a URL (in the style of Game Pigeon).

Single HTML file, no server needed.

As with everything on GitHub, use at your own risk. Not certified for any particular purpose.



## Highlander Chess Pro: Code review (ChatGPT and Claude) Update (5 November 2026)

**Scope:** A read-through review of the single-file HTML game, covering its JavaScript, browser storage, friend-game links, chat and computer opponent. It was done by an outside AI reviewer and then by Claude. It was not a hands-on attack test or a professional security audit.

**Overall:** No malicious behavior, credentials, trackers, outside scripts, or network requests were found. The game runs entirely in the browser with no server, and chat text is escaped before display. Most findings were low risk, but several were cheap to fix, so we fixed them.

### Findings and fixes

| Area | Finding | Fix |
|---|---|---|
| Friend links | A crafted link could be very large or damaged and freeze the page | Links over 5,000 characters, games over 1,000 moves, bad game IDs and undecodable data are rejected with "That link looks damaged" |
| Friend links | A link could include an "end Berserker turn" move when none was in progress, or moves after the game ended | Both are now rejected during replay |
| Friend links | A link could be sent in the middle of a Berserker chain | The send buttons are blocked until the turn is finished |
| Phone numbers | Saved in browser storage, which GitHub Pages shares across all projects under one `yourname.github.io` address | Saving is now opt-in. A Remember this number on this phone checkbox, plus a Forget saved data button |
| Saved games | Stored data was assumed to be well-formed | Validated on load, with a limit of 20 saved games. Damaged entries are skipped |
| Privacy | Links look private but aren't | A notice says links aren't encrypted and anyone who has one can read the game and your last message |
| Defence in depth | No browser policy limiting what the page can load or send | Added a Content Security Policy that blocks outside scripts and all network requests |

### Rules and engine fixes from the same review

- **Kings can no longer be captured.** Games end by checkmate.
- **Berserker exhaustion.** The third strike destroys the Berserker, and it can never strike a King.
- **New computer opponent.** It plays five levels, from Wee Bairn to Cromwell.
- **Same rules everywhere.** The computer, the board, undo and the friend-game replay use one set of rules.

### Testing performed

- **Rules match:** About 127,000 computer-played positions showed no differences between the new computer's legal moves and the game's own rules.
- **Strength:** At a tenth of a second per move, the new computer beat the old Hard level 11 games to 0, with 2 draws.
- **Legal moves only:** Every computer level played only legal moves in the finished page.
- **Link and storage hardening:** 15 automated checks passed, covering malformed links, oversized links, tampered saved data and normal games that replay exactly.
- **Balance:** More than 400 computer-vs-computer games measured results and showed no obvious color advantage. These were weak computers, so the numbers are indicative.

### Known gaps and open items

- **Bonnie Prince move:** The King and Queen swap isn't built yet.
- **Link versions:** Friend links carry no version number yet. Old test links from before the rule changes may show as damaged.
- **Threat previews:** The planned danger overlay and "pinned piece" hint aren't built yet.
- **Stalemate and quiet-move rules:** A stalemate is now a loss for the trapped side, and 60 quiet moves decides by material. The outside reviewer preferred standard chess rules here. We're keeping ours for now and will re-measure with the stronger computer.
- **Content Security Policy:** It is untested in a real browser. If sound, computer thinking or the friend panel stop working, loosen it.
- **Not an audit:** This was a source review. It isn't a guarantee against every vulnerability. Test a friend game on two real phones before sharing the game publicly.

### Planned

- **Shannon Level** A much harder level.

---
