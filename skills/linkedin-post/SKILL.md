---
name: linkedin-post
description: "Use when the user wants a LinkedIn post drafted, proofread, or saved — produces plain-text posts with blank lines between paragraphs in vaults/linkedin."
---

# LinkedIn post

Write the post, proofread it, save it as plain text. Nothing else.

## Steps

1. Draft the post from what the user gives you. If they give only a topic, write it; ask only when the angle, audience, or facts are genuinely missing.
2. Proofread: fix spelling, grammar, doubled words, punctuation, and inconsistent terms. Keep the author's voice, claims, paragraph order, and labels. Do not invent hashtags, emoji, or calls to action they did not ask for. Leave verbatim lines verbatim: sample captures, command lines, code, and quoted text keep their exact characters (`|`, `:`), even when they look like Markdown or typos.
3. Remove every Markdown artifact: no `#` headings, `**bold**`, `-` bullets, `>`, backticks, or `[text](url)` links. LinkedIn renders raw text, so those characters just show up. Use plain sentences; if a list genuinely helps, start each line with `•`, followed by a blank line between items. A bare `https://…` URL is fine — that is how references are written (step 5).
4. Separate every paragraph with exactly one blank line. No trailing spaces, no doubled blank lines, no blank line at the start or end. That spacing is what makes the post readable in the feed.
5. If the post has references (the repo, a post, a paper, a tool it talks about), put them at the bottom after one blank line following the closing paragraph, one per line, each as a short plain label and a bare URL — `Post: https://…`, `Source and docs: https://…`. No Markdown link syntax, no numbered list, no parentheses. Skip this when there is nothing to point at.
6. Keep it under 3,000 characters (LinkedIn's limit). If the draft is longer, cut the weakest paragraph instead of compressing every sentence.
7. Save with the `write` tool to `~/vaults/linkedin/YYYY-MM-DD-<slug>.txt`, creating the directory if missing. `<slug>` is 3–5 lowercase hyphenated words from the topic. Never save `.md`.
8. Show the user the final text and the file path.

If the user pastes an existing draft to fix, run steps 2–8 on it and change nothing they did not ask to change.

## Check before saving

- Extension is `.txt`; the content contains no Markdown.
- Paragraphs are separated by single blank lines.
- Reference lines, if any, sit at the bottom as `Label: URL`, one per line, with a single blank line above them.
- Proofread pass is complete: no typos, no doubled words, and the author's paragraphs, labels, and verbatim sample lines are unchanged except for fixed errors.
- Character count is under 3,000.

Verify in one pass before reporting:

```sh
f=~/vaults/linkedin/<file>.txt
wc -c "$f"                 # under 3000
grep -nE '^\s*(#|\*\*|-|>|`)|\[[^]]*\]\(|[ \t]+$' "$f"   # expects no matches
grep -c '^$' "$f"          # one blank line per paragraph gap
```

A grep hit inside a verbatim sample line (step 2) is expected — confirm the hit is quoted material, not formatting.
