---
title: "Using Claude Code as My Daily Code Assistant"
categories:
  - Blog
tags:
  - Claude
  - AI
  - Developer Tools
  - MCP
  - Automation
---

I’ve tried quite a few AI coding tools over time. Some were useful, some felt like autocomplete that talks too much.

Recently I started using Claude Code, and this is the first time it actually felt like a tool that fits into my workflow instead of interrupting it.

If you want to check it out or install it: https://docs.anthropic.com/

This isn’t a hype post. Just how I’ve been using it and what stood out.

---

## Claude Models vs Claude Code

This part may confuse a few initially.

Claude models are the actual AI models like Sonnet or Opus. They generate text, code, and explanations.

Claude Code is the developer tool built on top of those models. It lets you work inside your codebase, run commands, modify files, and interact with your project directly.

A simple way to think about it:
- Models do the thinking  
- Claude Code helps you apply that thinking inside your project  

Without Claude Code, you’re chatting. With it, you’re actually building.

---

## How I Use It Day to Day

Claude Code feels different because it works with your codebase instead of just answering questions.

Here are a few things I use often.

### `/` Commands

These are built-in commands.

Examples:
- `/help`
- `/fix`
- `/test`

Feels like giving instructions instead of writing prompts.

---

### `@` References

This is how you point Claude to context.

Examples:
- `@file.ts`
- `@folder/`
- `@package.json`

Instead of explaining everything, you just show it.

---

### `#` Instructions

Used to guide intent within the current session.

Example:
```
# Refactor this to improve readability and reduce duplication
```

This doesn’t get stored anywhere by default. It’s just a way to give structured guidance to Claude for that interaction.

If you want persistent behavior across sessions, that’s where `claude.md` comes in.

I usually use `#` for quick, one-off instructions and keep long-term rules in `claude.md`.

---

## Using `claude.md` and `claude.local.md`

This is one of my favorite parts.

### `claude.md`
- Lives in the repo  
- Shared across the team  
- Defines how Claude should behave consistently  

Example:
```markdown
- Follow clean code principles  
- Prefer simple solutions  
- Avoid unnecessary abstractions  
```

### `claude.local.md`
- Exists only on your machine  
- Not committed to the repo  
- Personal preferences  

Example:
```markdown
- Keep responses short  
- Skip long explanations unless asked  
```

This separation works really well. Team rules stay consistent, personal preferences stay local.

---

## MCP Integration

This is where things start getting interesting.

Claude Code supports MCP, which means it can connect to external tools in a structured way.

I’ve been experimenting with different MCP servers.

### Example: Playwright MCP

With Playwright MCP, Claude can:
- run browser tests  
- interact with UI  
- validate flows  
- debug frontend issues  

Instead of writing test scripts manually, you can do something like:

```
Test the login flow and identify failures
```

And it actually runs it.

At that point, it’s not just suggesting code. It’s doing work.

---

## The `.claude` Folder

Claude Code also uses a `.claude` directory for configuration.

You can define things like:
- behavior  
- rules  
- hooks  

### Example: Hook

You can set up a hook that runs after changes.

For example:
- after code changes, run tests  
- after generating code, run lint  

It’s a simple way to automate small checks without thinking about them every time.

---

## Using Claude with GitHub

This is a workflow I’ve been using recently.

1. Use Claude Code locally to:
   - write or refactor code  
   - generate tests  

2. Push changes to GitHub  

3. Use Claude again for:
   - reviewing changes  
   - suggesting improvements  
   - checking for issues  

With an API key, you can integrate it into PR reviews or CI flows.

It doesn’t replace reviews, but it speeds them up.

---

## Final Thoughts

Claude Code feels less like a chatbot and more like a tool that actually sits in your workflow.

It doesn’t remove thinking. It just removes some of the friction around doing things.

Once you start using it with MCP, it goes beyond writing code and starts helping with execution too.

Still early, but definitely useful.

---

## Over to You

If you want to explore this properly, I’d recommend:

"Claude Code in Action" by Anthropic  

It helped me understand what’s possible and how to use it in a more structured way.

If you’ve been using Claude Code or experimenting with MCP, I’m curious how you’re using it.
