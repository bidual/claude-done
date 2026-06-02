# Install prompt

This whole file is a prompt. Paste it into Claude Code, or just point Claude at it and say "follow this." Everything below the line is written to Claude.

---

You are installing a Stop hook for Claude Code. It prints one line with the current time every time you finish a turn, so the user, who keeps several Claude sessions running at once, can tell when each one stopped.

Settle the wording and the time format together with the user before you edit anything (step 1). Do not just pick a default and write it in.

## 1. Let the user choose the line

Do not pick this silently, and do not hand them a fixed menu of languages. Propose one sensible default, in the language the user writes to you in, then let them confirm it or type the exact line they want to see.

Build the default from what you have: the `language` field in `~/.claude/settings.json`, any message an existing hook already prints, their CLAUDE.md, and the language they are writing to you in right now. Show it as a starting point, for example `완료: 2026년 06월 03일 14:30`, and ask them to keep it or type their own.

Whatever they give you is the target: their language, script, digits, date order, and choice of 12 or 24 hour. Do not move on until you have a specific line they approved.

## 2. Build a command that reproduces their line

The hook is one shell command whose output is the approved line, as JSON:

```
date +'{ "systemMessage": "<their line>" }'
```

Their line shows one moment as an example, so map its date and time parts to `date` codes and it will keep updating. Numeric codes do not depend on a locale, so use them for the numbers: `%Y` year, `%m` month, `%d` day, `%H:%M` 24-hour time, `%S` seconds, `%I:%M %p` for a 12-hour clock.

A month name (`%b`, `%B`) follows `LC_TIME`, and the hook runs in an environment that is usually not the user's language, so pin the locale in the command (`LC_TIME=de_DE.UTF-8 date +...`) or `%B` comes out in English. If their line needs something plain `date` cannot produce, like non-Latin digits or a non-Gregorian calendar, post-process the output to match it (digits transliterate with `sed`), or tell them `date` cannot and agree on an alternative.

Keep a `"` or a `\` out of the output. Either one breaks the JSON.

## 3. Test it before you save it

Run the exact command you will save, including any `LC_TIME=` prefix, and check two things:

```
date +'{ "systemMessage": "..." }' | jq .
```

First, `jq` accepts it, so the JSON is valid. Second, which `jq` does not check, the rendered text matches the line the user approved, exactly: right month, right digits, right order, nothing doubled. Read it yourself. If it does not match, fix it and run it again.

## 4. Merge it into settings

Read `~/.claude/settings.json` and add a `Stop` entry under `hooks`, leaving anything already there alone:

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          { "type": "command", "command": "date +'{ \"systemMessage\": \"Done: %d %b %Y, %H:%M\" }'" }
        ]
      }
    ]
  }
}
```

The inner quotes are escaped (`\"`) because the command is itself a JSON string. If there is already a `Stop` array, add to it instead of overwriting. If other events like `PreToolUse` or `SessionStart` are there, leave them be.

## 5. Validate the file

```
jq -e '.hooks.Stop[].hooks[].command' ~/.claude/settings.json
```

Exit code 0 with your command printed back means the JSON parses. A broken `settings.json` silently turns off every setting in the file, so do not skip this.

## 6. Let the user know it's live

Tell them it starts on the next turn, that `/hooks` is where they view, edit, or disable it, and which `date` field to change if they want a different format later.

The hook runs when the current turn ends, so the first stamp lands right after your install message. If nothing shows up, the settings watcher probably did not catch the new file. Have them open `/hooks` once to reload, or restart Claude Code.
