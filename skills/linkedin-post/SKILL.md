---
name: linkedin-post
description: "Use when the user wants a LinkedIn post drafted, proofread, or saved — produces plain-text posts with blank lines between paragraphs in vaults/linkedin."
---

# LinkedIn post

Write the post, proofread it, save it as plain text. Nothing else.

## Steps

1. Draft the post from what the user gives you. If they give only a topic, write it; ask only when the angle, audience, or facts are genuinely missing.
2. Proofread: fix spelling, grammar, doubled words, punctuation, and inconsistent terms. Keep the author's voice and claims. Do not invent hashtags, emoji, or calls to action they did not ask for.
3. Remove every Markdown artifact: no `#` headings, `**bold**`, `-` bullets, `>`, backticks, or `[text](url)` links. LinkedIn renders raw text, so those characters just show up. Use plain sentences; if a list genuinely helps, start each line with `•`, followed by a blank line between items. A bare `https://…` URL is fine — that is how references are written (step 5).
4. Separate every paragraph with exactly one blank line. No trailing spaces, no doubled blank lines, no blank line at the start or end. That spacing is what makes the post readable in the feed.
5. If the post has references (the repo, a post, a paper, a tool it talks about), put them at the bottom as bare URLs, one per line, after one blank line following the closing paragraph. No labels, no numbered list, no parentheses — just the URL on its own line. Skip this when there is nothing to point at.
6. Keep it under 3,000 characters (LinkedIn's limit). If the draft is longer, cut the weakest paragraph instead of compressing every sentence.
7. Save with the `write` tool to `~/vaults/linkedin/YYYY-MM-DD-<slug>.txt`, creating the directory if missing. `<slug>` is 3–5 lowercase hyphenated words from the topic. Never save `.md`.
8. Show the user the final text and the file path.

If the user pastes an existing draft to fix, run steps 2–8 on it and change nothing they did not ask to change.

## Check before saving

- Extension is `.txt`; the content contains no Markdown.
- Paragraphs are separated by single blank lines.
- Reference URLs, if any, sit at the bottom, one per line, with a single blank line above them.
- Proofread pass is complete: no typos, no doubled words.
- Character count is under 3,000.
