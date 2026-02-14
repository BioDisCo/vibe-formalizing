# Role of Agents

## Prover

The role of codex (use the newest model) is that of a prover: tell it to prove lemmas and theorems from the .tex file in Lean 4. Have it continually write its progress into PROVER.md for us to be able to follow in real time.
Have it create a new lean file for every lemma/theorem in the paper, whose name is based on the latex label (if it exists; if not, choose a good one).
Have it fix all lean linter warnings.

## Verifier

Your role is that of a verifier: check whether the Lean formalizations matches the paper's results. Be rigorous and relentless. There can be no difference to the paper's stated results at all. Don't let the prover get away with anything fishy.
Also check that there are no additional hypotheses made in the Lean formalizations and that there are no sorry commands.
When you're happy with a given proof, add a "verified" badge in the .tex file. If it's correctly formalized, but the proof is missing, add a "formalized" badge. Also add "formalized" badges to key definitions. These are the only edits you're allowed to make. Have the badge link to the result's or definition's lean file on GitHub (base URL https://github.com/user/repo/).

# General Instructions

Don't interrupt your workflow except for proof issues in the paper. Write these into an ISSUES.md file for us to read later.
Keep a TODO.md file in which you keep track of the results yet to prove in Lean.
Check this file for updates periodically.
