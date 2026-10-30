# Claude Code status line prompt

 Paste it into Claude Code, after /statusline

```text
Build me a Claude Code status line as a bash script (statusline-command.sh in my
Claude config dir), wired in settings.json as
{"statusLine": {"type": "command", "command": "bash <path>/statusline-command.sh"}}.
Read the stdin JSON with jq using `// empty` so missing fields never print "null".
One line, segments separated by a dim │, ANSI colors, no powerline glyphs,
no network calls, must run in well under 100ms. Test it with sample JSON before finishing.

Segments, in order (omit any segment whose data is missing):
1. Folder: basename of workspace.current_dir (bold cyan), then the git branch (magenta).
2. Git state: yellow ● if uncommitted changes, green ↑N ahead / red ↓N behind upstream
   (git --no-optional-locks).
3. Jira ticket parsed from the branch name, e.g. XX-1234-foo → XX-1234 (bold blue).
4. AWS: `aws:<AWS_PROFILE without the .RoleName suffix>`; bold red if the profile name
   contains "prod", orange otherwise. Look up the profile's sso_start_url in ~/.aws/config,
   find the matching token in ~/.aws/sso/cache/*.json, and show time left (⏳3h20m):
   dim normally, yellow under 30 min, red "SSO expired" after.
5. GitHub: active gh account read from ~/.config/gh/hosts.yml (not `gh auth status`, too slow).
   Show red "gh:<user> ⚠" if it doesn't match the folder:
   <~/work-folder> → <work-github-user>, <~/personal-folder> → <personal-github-user>.
6. Model: model.display_name (blue).
7. Effort: effort.level, falling back to effortLevel in settings.json.
8. Context: "ctx" + 10-cell bar ▓░ + percent from context_window.used_percentage.
9. Plan usage: "5h" and "7d" bars from rate_limits.five_hour / seven_day.used_percentage,
   each followed by a dim ↻ time-until-reset from resets_at (e.g. ↻2h13m, ↻3d11h).
10. Cost: cost.total_cost_usd as $0.00.

All bars: green <50%, yellow 50–80%, red >80%.
```
