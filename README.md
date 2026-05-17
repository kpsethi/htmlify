# htmlify skill

A Claude Code skill that generates rich, self-contained HTML for any topic and publishes it to [htmlify.vercel.app](https://htmlify.vercel.app) for sharing.

## What it does

HTML is a better medium than markdown for communicating complex information. It carries color, layout, SVG diagrams, interactive tabs, and syntax-highlighted code — all in a single file. This skill makes Claude generate that HTML for you.

```
/htmlify teach me how React's useEffect works
```

Claude generates a visual HTML lesson with analogies, diagrams, and code samples — opens it in your browser. Iterate until it's right, then:

```
/htmlify publish
```

Uploads it anonymously, hands you a shareable URL.

## Install

Copy `SKILL.md` to your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/htmlify
cp SKILL.md ~/.claude/skills/htmlify/SKILL.md
```

Restart Claude Code. The `/htmlify` skill is now available.

## Examples

```
/htmlify teach me Flutter widgets
/htmlify explain how RLHF works
/htmlify turn these notes into a visual summary
/htmlify make a slide deck on system design principles
/htmlify publish
```

## Web app

The publish action posts to [github.com/kpsethi/htmlify](https://github.com/kpsethi/htmlify) — a minimal Next.js app that hosts the HTML. Self-host it if you want your own URL.
