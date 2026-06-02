# claude-done

A timestamp on every finished Claude Code turn, so you can tell at a glance which tab just wrapped up.

[한국어](README.ko.md) · [日本語](README.ja.md) · [中文](README.zh.md) · [Español](README.es.md)

```
Done: 03 Jun 2026, 03:39
```

## Why

If you only run one session, you don't need this. You're sitting right there watching it run.

It only starts to pay off once you've got five or six running across tabs. You kick them off and go get coffee. Come back and the transcripts all look the same. Nothing tells you whether a tab finished ten seconds ago or has been sitting idle for twenty minutes.

So claude-done prints the time at the end of every turn. One look and you know which tabs just finished. That's all it does. It's one extra line per turn, nothing to manage.

## What you get

| Locale | Line |
|--------|------|
| English | `Done: 03 Jun 2026, 03:39` |
| 한국어 | `완료: 2026년 06월 03일 03:39` |
| 日本語 | `完了: 2026年06月03日 03:39` |
| Español | `Hecho: 03 jun 2026, 03:39` |
| Deutsch | `Fertig: 03.06.2026, 03:39` |

The word and the time format follow your locale. Claude checks your settings and suggests a matching format. Change it to whatever you like.

## Install

Paste this into Claude Code:

```
Set up the stop hook from github.com/bidual/claude-done.
Read its INSTALL.md and follow it.
```

Claude figures out your language and time format, writes the hook into `~/.claude/settings.json`, and confirms it actually runs. It takes effect on the next turn. [INSTALL.md](INSTALL.md) is a prompt, not a binary, so nothing is hidden. Read it first if you want to know exactly what gets added.

## How it works

The hook is a single command:

```
date +'{ "systemMessage": "Done: %d %b %Y, %H:%M" }'
```

Claude Code fires a Stop hook after every turn, and that's this command. `date` fills in the time and prints a small JSON object. Claude Code reads the `systemMessage` field and shows it. Nothing runs in the background.

The format is yours. These are standard `date` codes (`man date`), so swap them however you like: `%S` for seconds, `%I:%M %p` for a 12-hour clock, and so on.

## Changing or removing it

Run `/hooks` inside Claude Code to view, edit, or disable the hook. You can also edit `hooks.Stop` in `~/.claude/settings.json` directly. To remove it, delete the Stop block or just ask Claude to take it out.

## License

[MIT](LICENSE)
