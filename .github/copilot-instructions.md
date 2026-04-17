# Copilot Instructions — Greek Flashcard Word Repository

## Project Overview

This workspace maintains a structured repository of Greek vocabulary flashcards for a custom flashcard application. The workflow is:

1. **Sources** arrive in `sources_gitignore/` (never committed to git).
2. **You analyze** every source, extract all vocabulary, and produce well-structured `.md` files.
3. **Output files** live in `words/` inside this repository (`my_greek_words/words/`). Create nested subdirectories freely.
4. The flashcard app reads all `.md` files from `words/**/*.md` recursively.

> Rule zero: **never commit** anything inside `sources_gitignore/`. It is gitignored.

---

## Workspace Layout

```
my_greek_words/               ← git repo root (ALL output goes here)
  .github/
    copilot-instructions.md   ← this file
  .gitignore
  sources_gitignore/          ← local sources only (gitignored, never commit)
    Чат с учителем WhatsApp/
    Уроки …/
    …
  words/                      ← OUTPUT: all flashcard .md files go here
    01_basics.md
    02_letters_words.md
    grammar/                  ← nested subdirectories are allowed and encouraged
      articles.md
      pronouns.md
      prepositions.md
    adjectives/
      colors.md
      size_shape.md
      emotions.md
    verbs/
      reflexive.md
      motion.md
    nouns/
      body.md
      animals.md
      food_kitchen.md
      nature.md
      home.md
    phrases/
      greetings.md
      small_talk.md
      self_introduction.md
```

---

## Output File Format

Always follow the word card format. Summary:

```markdown
# Greek word / phrase (with article for nouns)
## Russian translation
### Latin transcription with stress marks
Optional extra content (conjugations, declensions, examples, notes)
```

### Mandatory rules

| Rule | Detail |
|------|--------|
| **Article on nouns** | Always include the definite article: `# ο άντρας`, `# η γυναίκα`, `# το παιδί` |
| **Verb conjugation** | Three tenses in bullet lists: present → aorist → future |
| **Adjective forms** | All three genders + plurals: `μεγάλος(-η,-ο)` |
| **Participle / verbal adj.** | Same as adjectives |
| **Stress marks** | Transcription must carry stress: `kalimér**a**` → `kaliméra` |
| **No blank lines inside a block** | No empty lines between `#`, `##`, `###` of the same word |
| **Empty line between blocks** | Exactly one blank line between different word blocks |
| **UTF-8 encoding** | All files must be UTF-8 |
| **No duplicates** | Before adding a word, search all existing `.md` files. If already present — skip |

### Nested folder structure

Use subdirectories in `words/` when a topic group is large or distinct:

```
words/
  grammar/
    articles.md
    pronouns.md
    prepositions.md
  adjectives/
    colors.md
    size_shape.md
    emotions.md
  verbs/
    motion.md
    reflexive.md
  nouns/
    body.md
    food_kitchen.md
    animals.md
    nature.md
    home.md
    city.md
  phrases/
    greetings.md
    small_talk.md
    self_introduction.md
```

Flat files (`01_basics.md`, `02_letters_words.md`) already exist — keep them; do NOT rename or restructure them. Add new content only in new files or new subdirectory files.

---

## Source Types and How to Process Each

### 1. WhatsApp Chat (`_chat.txt`)

**Format:** `[DD/MM/YYYY, HH:MM:SS] ~Anastasia: <text>`

**What to extract:**
- Any standalone Greek word or phrase posted by the teacher (~Anastasia) — these are active lesson vocabulary.
- Grammar snippets (e.g. `Μου - мой, моя, мое`, conjugation tables, declension patterns).
- Example sentences in Greek (even mid-conversation).
- Vocabulary pairs mentioned in Russian → Greek direction (e.g. `Дети - τα παιδιά`).

**Ignore:**
- Pure Russian/logistics messages (scheduling, greetings in Russian).
- `<attached: …>` image references (images are handled separately).
- `‎This message was deleted.`
- Links (e.g. URLs).

**Algorithm:**
1. Scan every ~Anastasia message.
2. If the message is a Greek word/phrase: add to word list with its part of speech.
3. If the message contains a Russian gloss on the same line or adjacent line: record as translation.
4. If the message is a conjugation/declension table: attach as extra content to the relevant word card.
5. If the message is a full example sentence: attach as a usage example (`•` bullet).

### 2. PDF Lesson Files (`*.pdf`)

**What to extract:**
- Vocabulary lists.
- Grammar tables (articles, verb paradigms, declensions).
- Dialogues — break into individual words and phrases.
- Exercise prompts that contain Greek text.

**How to process:**
- Read the PDF content carefully.
- Identify the lesson topic (from filename or first page).
- Map words to the appropriate thematic `assets/words/` file.
- For grammar patterns found in PDFs, create or update a file under `assets/words/grammar/`.

### 3. Photos of Handwritten Exercises (`*.jpg` / `*.png`)

**What to extract:**
- Any Greek text visible in the photo (words, sentences, conjugation tables, teacher corrections).
- Pay attention to corrections — the corrected form is the correct one.

**How to process:**
- Treat exactly the same as plain text: extract each identifiable Greek token.
- If the photo shows a full sentence, add the sentence as a usage example (`•`) under the relevant word.

---

## Word Extraction Rules

### From a single Greek token (word or short phrase)

1. Identify part of speech (noun / verb / adjective / adverb / pronoun / preposition / phrase).
2. Look up or infer the full canonical form:
   - Nouns → nominative singular with article.
   - Verbs → 1st person singular present.
   - Adjectives → masculine nominative singular, add `(-η,-ο)` suffix or full paradigm.
3. Generate Russian translation.
4. Generate Latin transcription with stress marks.
5. Add conjugation / declension / forms in the extra block.
6. Add 1–3 example sentences where possible.

### From a Greek sentence

1. Parse the sentence into individual words.
2. For each word NOT already in the repository: create a new word card.
3. Add the full sentence as a usage example (`•`) under the most relevant word.
4. If the sentence illustrates a grammar point, note it in the extra block.

### Grammar patterns

When a source contains a grammar table or paradigm (e.g. possessive pronouns, article declension, verb class paradigm), create a dedicated card:
- `#` heading = the pattern name / lemma (e.g. `# μου` for possessive pronouns)
- Extra block = full paradigm table in plain text or bullet list.

---

## Deduplication Protocol

Before adding any word:

1. Search all `.md` files in `words/**` for the exact Greek form (H1 `# …`).
2. Also check common variants (capitalized first letter, with/without article).
3. If a word already exists — do NOT add it again. You may enrich the existing card (add examples, improve conjugation) but do not create a duplicate entry.
4. Track processed sources: add a comment at the top of new `.md` files listing which sources were mined, e.g.:

```markdown
<!-- Sources: WhatsApp chat 2026-03-29 – 2026-04-17, Урок 5.1 PDF -->
```

---

## File Naming Convention

```
NN_topic.md          — top-level files, NN = two-digit sequential number
subdirectory/topic.md — no number prefix needed inside subdirectories
```

Examples:
- `03_adjectives.md` — if a flat adjective file is needed
- `adjectives/colors.md` — preferred for larger topic groups
- `grammar/reflexive_verbs.md`
- `nouns/animals.md`

---

## Step-by-Step Workflow When New Sources Arrive

1. **List new files** in `sources_gitignore/` that have not been processed yet.
2. **Read each source** fully before writing any output.
3. **Collect all Greek items** into a temporary internal list grouped by topic.
4. **Deduplicate** against existing `words/**/*.md`.
5. **Determine target files**: match each topic group to an existing file or decide on a new file/subdirectory.
6. **Write or append** the word blocks. New files get the source comment header. Appended words go at the end of the file.
7. **Validate** the result: check format compliance (H1→H2→H3 order, no empty lines inside blocks, article on nouns, conjugation on verbs, adjective forms).
8. **Report** to the user: list of new files created, files updated, and count of words added.

---

## Quality Standards

- **Completeness**: extract every identifiable Greek token from the source — do not skip words because they seem "too simple" or "already obvious".
- **Accuracy**: verify transcription stress marks. If unsure, mark with `(?)`.
- **Examples**: always include at least one example sentence per word if one appears in the source; add a natural one if none is given.
- **Conjugations**: for every verb, provide all three tenses (present, aorist, future) as bullet lists — this is mandatory per `how_to_build_words.md`.
- **Adjective forms**: always provide masculine / feminine / neuter in singular and plural.
- **Consistency**: use the same formatting style as existing files (`01_basics.md`, `02_letters_words.md`).

---

## Existing Content Reference

Current topics already covered (do not duplicate these words):

| File | Coverage |
|------|----------|
| `words/01_basics.md` | Greetings (καλημέρα, γεια σου…), question words (Πώς, Πόσο, Πού, Τι, Γιατί, Πότε, Ποιος/α/ο/ν), state replies (μια χαρά, πολύ καλά, χάλια…), basic verbs (κάνω, έχω, βλέπω, ξέρω…) |
| `words/02_letters_words.md` | Core verbs with full conjugation: είμαι, μου αρέσει, περνάω, διασκεδάζω, αγοράζω, λειτουργώ, προσπαθώ, επικοινωνώ, παίρνω, … |

Always grep both files before adding a verb or common phrase.

---

## Example: Processing a WhatsApp Batch

Given chat entries (teacher messages only):
```
Κόκκινος
Πράσινος
Γαλάζιος
Λευκός
Άσπρος
Το χρώμα
Χρωματιστός
```

**Output** → new file `words/adjectives/colors.md`:

```markdown
<!-- Sources: WhatsApp chat 2026-04-03 -->

# κόκκινος(-η,-ο)
## красный
### kókkinos(-i,-o)
**Муж.:** κόκκινος / κόκκινοι
**Жен.:** κόκκινη / κόκκινες
**Ср.:** κόκκινο / κόκκινα

• Το μήλο είναι κόκκινο. — Яблоко красное.

# πράσινος(-η,-ο)
## зелёный
### prásinos(-i,-o)
**Муж.:** πράσινος / πράσινοι
**Жен.:** πράσινη / πράσινες
**Ср.:** πράσινο / πράσινα

• Το δέντρο είναι πράσινο. — Дерево зелёное.

# γαλάζιος(-α,-ο)
## голубой, лазурный
### galázios(-a,-o)
**Муж.:** γαλάζιος / γαλάζιοι
**Жен.:** γαλάζια / γαλάζιες
**Ср.:** γαλάζιο / γαλάζια

• Η θάλασσα είναι γαλάζια. — Море голубое.

# λευκός(-ή,-ό)
## белый (нейтральный)
### lefkós(-í,-ó)
**Муж.:** λευκός / λευκοί
**Жен.:** λευκή / λευκές
**Ср.:** λευκό / λευκά

• Το χιόνι είναι λευκό. — Снег белый.

# άσπρος(-η,-ο)
## белый (разговорный)
### áspros(-i,-o)
Разговорный синоним λευκός, употребляется чаще в повседневной речи.
**Муж.:** άσπρος / άσπροι
**Жен.:** άσπρη / άσπρες
**Ср.:** άσπρο / άσπρα

• Άσπρο σπίτι. — Белый дом.

# το χρώμα
## цвет
### to hróma
ед.ч.: το χρώμα / του χρώματος
мн.ч.: τα χρώματα / των χρωμάτων

• Τι χρώμα είναι αυτό; — Какой это цвет?

# χρωματιστός(-ή,-ό)
## цветной, разноцветный
### hromatistós(-í,-ó)
**Муж.:** χρωματιστός / χρωματιστοί
**Жен.:** χρωματιστή / χρωματιστές
**Ср.:** χρωματιστό / χρωματιστά

• Φοράει χρωματιστά ρούχα. — Он/она носит цветную одежду.
```

---

## Quick Reference: Parts of Speech Markers

| Type | What to include in extra block |
|------|---------------------------------|
| Noun | Article + singular/plural forms (nominative + genitive) |
| Verb | Present / Aorist / Future conjugation tables (all 6 persons) |
| Adjective | M/F/N forms in singular and plural |
| Adverb | Usage note + 1–2 examples |
| Pronoun | Full paradigm if it's a pronoun class (possessive, personal…) |
| Preposition | All common contractions (e.g. `σε + το = στο`) + examples |
| Phrase | Register note (formal/informal) + literal translation if non-obvious |
