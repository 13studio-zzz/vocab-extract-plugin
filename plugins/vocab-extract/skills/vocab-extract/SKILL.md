---
name: vocab-extract
description: >
  Extracts bold English vocabulary words/phrases and their Korean meanings from vocabulary
  textbook images and outputs a clean markdown table. Use this skill whenever the user
  attaches one or more vocabulary images (textbook screenshots, vocabulary list photos,
  word list images) and asks to extract words, make a table, build an Excel sheet, or
  organize vocabulary. Trigger even if the user just says "이미지에서 단어 뽑아줘",
  "표로 만들어줘", "엑셀로 만들어줘", or pastes vocabulary images without much explanation.
  This skill is especially valuable for Korean English-learner vocabulary books where
  bold words appear alongside Korean definitions.
---

# Vocabulary Extractor

Your job is to read vocabulary images and extract bold English words with their Korean
meanings into a markdown table. Precision matters: you are the source of truth, so read
carefully and extract completely.

## How to read the images

Scan the image from top to bottom, line by line. For each line, ask: **is the English
text bold and black?** If yes, extract it along with the Korean meaning to its right.

Bold text in these vocabulary books typically appears heavier/darker than surrounding
text. Common patterns:

- A single bold word: `price` → `가격, 가치(를 매기다)`
- A bold phrase: `be ready to V` → `기꺼이 ~하다`
- A bold phrase spanning multiple words: `in addition to` → `~뿐만 아니라`

## What to include

Extract **all** bold English entries — even if they appear indented or seem like
sub-entries. Indentation is irrelevant. If it's bold, extract it.

## What to exclude

Skip non-bold (light/thin) entries — these are derivatives, synonyms, or related forms
that are intentionally de-emphasized (e.g., `appreciation`, `ending`, `fully`).

## Parentheses for extra info

If there is additional information next to the Korean meaning — such as verb conjugations
written in green or small text, irregular forms, or part-of-speech markers — include it
at the end of the meaning field inside parentheses.

Example: `멈추다, 잡다, 갖다 (hold held held)`

## Output

Output **only** the markdown table below. No introduction, no explanation, no summary.
Nothing before or after the table.

```
| 영단어/숙어 | 뜻 |
|---|---|
| price | 가격, 가치(를 매기다) |
| praise | 칭찬, 찬양(하다) |
| be ready to V | 기꺼이 ~하다 |
```

If the user provides multiple images, process all of them in sequence and combine into
one table in the order they appear.
