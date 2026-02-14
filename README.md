# vibe-formalizing

This is our setup for formalization of our papers from Theoretical Computer Science (TCS).
The idea is to use agent AI to verify (some of) the proofs in our papers.
For the lemmas or theorems where we are confident that a formal proof has been obtained, we add a badge to the tex code that links to the formalization.
Currently we use Lean as a theorem proofer.
See this [short YouTube video](https://youtu.be/rE5_Zc2AMwM) for an example.

The setup uses Claude Code and Codex via an MCP server for theorem proving and computational verification. We're constantly improving this workflow and would love to hear your thoughts on how we can improve it. Feel free to share feedback, ideas, or issues as you explore!

## Getting Started

Start by installing the three core pieces: **Lean 4**, **Claude Code**, and **Node.js** (v18+).

For Lean, head to [lean-lang.org](https://lean-lang.org/). Claude Code installs via `npm install -g @anthropic-ai/claude`, or grab the latest [release](https://github.com/anthropics/claude-code/releases) if you prefer.

Once you have those, clone this repository and install dependencies:

```bash
git clone https://github.com/BioDisCo/vibe-formalizing.git
cd vibe-formalizing
npm install
```

Copy or clone your paper (with LaTeX source) into this directory. Both Claude and the Codex agent will need access to the files to work effectively on formalization:

```bash
cp -r /path/to/your/paper .
```

You'll also want to clone the [verified-badges](https://github.com/BioDisCo/verified-badges) repository into this directory. It contains LaTeX code for adding verification badges to your paper, plus tools for generating badges for new theorem provers:

```bash
git clone https://github.com/BioDisCo/verified-badges.git
```

## Connecting Codex

The real magic happens when you add the Codex MCP server. This plugs advanced mathematical reasoning directly into Claude:

```bash
claude code mcp add codex -s user -- codex \
  -m gpt-5.3-codex \
  -c model_reasoning_effort="high" \
  mcp-server .
```

This sets up a powerful reasoning backend that helps with proofs, verification, and code generation. You can verify everything connected by running `mcp --list-servers`.

## Using It

Start a session with `claude`. When Claude connects, give it this initial prompt to get oriented:

```
Follow the instructions in INSTRUCTIONS.md
```

This tells Claude to read the project-specific guidance and context. From there, Claude will generate a `TODO.md` and ask Codex to formalize a lemma or theorem in the paper. Codex reports on progress continuously in a status file. Once done with a statement, it asks Claude to review the formalization - in particular the consistency of the statement with the paper. Test is against Lean, which on success of Lean and the correspondence report by Claude, adds a badge to the paper tex.

Before diving into a task, check the markdown files in this repository; they'll give you context and guidance on what's in progress.

You may also want to use `tail -F` to live view todo and report files during the potentially long back and froth between the agents.

## Configuration

Your MCP settings live in `~/.claude/config.json`. If you need to tweak reasoning effort or other parameters, edit that file or just re-run the `mcp add` command.

Want to customize keybindings? Try `claude code /keybindings-help`.

## If Something Breaks

**MCP server not showing up?** Run `claude code --debug` and check `mcp --list-servers`. If it's really stuck, remove and re-add:
```bash
claude code mcp remove codex
claude code mcp add codex -s user -- codex -m gpt-5.3-codex -c model_reasoning_effort="high" mcp-server .
```

**Lean missing?** Make sure it's in your PATH with `which lean` and `lean --version`.

We'd love to hear about your experience with this setup—what works well, what's clunky, and what you'd like to see improved. Your feedback helps us refine this workflow for everyone.

## Resources

- [Lean 4 Documentation](https://lean-lang.org/lean4/doc/)
- [Claude Code Repository](https://github.com/anthropics/claude-code/)
- [Verified Badges Repository](https://github.com/BioDisCo/verified-badges)
