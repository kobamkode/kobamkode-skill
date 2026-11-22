---
name: linkedin-post
description: "Use when the user wants a LinkedIn post drafted, proofread, or saved — produces plain-text posts with blank lines between paragraphs in vaults/linkedin."
---

# LinkedIn post

Write the post, proofread it, save it as plain text. Nothing else.

## Steps

1. Draft the post from what the user gives you. If they give only a topic, write it; ask only when the angle, audience, or facts are genuinely missing.
2. Proofread: fix spelling, grammar, doubled words, punctuation, and inconsistent terms. Keep the author's voice and claims. Do not invent hashtags, emoji, or calls to action they did not ask for.
3. Remove every Markdown artifact: no `#` headings, `**bold**`, `-` bullets, `>`, backticks, or `[text](url)` links. LinkedIn renders raw text, so those characters just show up. Use plain sentences; if a list genuinely helps, start each line with `•`, followed by a blank line between items.
4. Separate every paragraph with exactly one blank line. No trailing spaces, no doubled blank lines, no blank line at the start or end. That spacing is what makes the post readable in the feed.
5. Keep it under 3,000 characters (LinkedIn's limit). If the draft is longer, cut the weakest paragraph instead of compressing every sentence.
6. Save with the `write` tool to `~/vaults/linkedin/YYYY-MM-DD-<slug>.txt`, creating the directory if missing. `<slug>` is 3–5 lowercase hyphenated words from the topic. Never save `.md`.
7. Show the user the final text and the file path.

If the user pastes an existing draft to fix, run steps 2–7 on it and change nothing they did not ask to change.

## Check before saving

- Extension is `.txt`; the content contains no Markdown.
- Paragraphs are separated by single blank lines.
- Proofread pass is complete: no typos, no doubled words.
- Character count is under 3,000.
