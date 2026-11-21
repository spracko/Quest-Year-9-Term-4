# Quest game: tailoring guide for other year groups

This guide explains how to turn the Year 9 game **Atoms & Earth Quest** into a version for another class. It is written so that a teacher, or Claude working in another project, can follow it step by step.

Upload these three files into the new project:

1. `Atoms and Earth Quest.html`: the game. It is one self-contained file that works offline in any browser.
2. `Atoms and Earth Quest - question bank.txt`: the Year 9 questions. Use them as a model for the format and style, not as content.
3. This guide.

---

## 1. How the game works (so you know what you are changing)

- Players cross three lands (woods, dry plains, snow), then fight a boss in its lair.
- **Creatures** chase the player. When caught, the player answers a question. A right answer makes the creature flee. A wrong answer starts a real-time fight: ← and → move in and out, and Space strikes.
- **Supplies** (food, water, shelter, first aid) ask a question. A right answer uses the item. About 1 in 5 is secretly poisoned and takes health instead of giving it. A wrong answer leaves the item behind.
- **Scrolls** (3 per land) ask harder questions and open the gate.
- **The boss** asks a question. A correct answer drops its shield for a few seconds so the player can attack. A wrong answer means the boss attacks.
- There are **three question types**: multiple choice, vocabulary (read a definition and type the term) and numerical (type a number).
- A **report card** (R key or button) pauses the game and shows questions asked and answered correctly, by type and by chapter. It also appears on the end screen, together with every missed question and its answer.
- There are **four difficulty levels**: Explorer, Adventurer, Legend and Nightmare. Harder levels give less health, more and faster creatures, fewer supplies, harder questions, two correct answers in a row to escape, and a tougher boss.

## 2. Where things live in the HTML file

Open the HTML in any text editor. Near the top of the `<script>` section there are three blocks:

| Block | What it controls |
|---|---|
| `const GAME = { … }` | Title, year label, chapters/modules, creature names, land names and descriptions, scroll name, question mix, default difficulty |
| ``const DEFAULT_BANK = ` … `;`` | Every question, one per line |
| `const DIFFS = [ … ]` (further down) | Difficulty settings: health, speed, number of supplies, poison chance, boss strength |

Also change the `<title>…</title>` line on the first lines of the file. It is the browser-tab name and must match `GAME.title`.

You never need to touch the drawing, movement or fight code.

**Don't add HTML `<form>` elements.** Some viewers (including shared Claude links) block forms, which once stopped the typed-answer Check button from working. The game uses a plain button and an Enter-key listener instead. Keep it that way if you change anything.

---

## 3. Step-by-step tailoring checklist

### Step 1: Gather the content
- [ ] The textbook text or notes for each chapter or module, with its **key terms and definitions** and important numbers (dates, percentages, constants, values in worked examples).
- [ ] The syllabus outcomes or learning intentions, so questions match what is taught.
- [ ] The class's **assessment tasks and exam papers**. Keep game questions different from assessed items, so the game is practice and doesn't leak the exam.
- [ ] Which sections students have actually covered so far. Leave out content they haven't met yet, or add it later.

### Step 2: Edit the `GAME` settings
- [ ] `title`: a new name tied to the topic, two to four words (e.g. "Cells & Forces Quest"). Change `<title>` at the top of the file to match.
- [ ] `eyebrow`: e.g. `'Year 7 Science · Chapters 5 & 6'`.
- [ ] `chapters`: one entry per chapter or module. Each `id` must match the first column of the questions. `label` appears on badges and the report card, and `menu` is the start-screen button. One chapter hides the chapter chooser. Two or more add an "all" option plus one button per chapter. Up to four works well.
- [ ] `saveKey`: a short unique word for this version (e.g. `'y7quest'`). This keeps teachers' edited banks and sound settings separate between versions on the same computer.
- [ ] `creatures`: rename all six to suit the topic, and give each a matching attack verb. **The pictures don't change**, so pick names that fit the picture (see the comments beside each one). The `dragon` is the boss. `short` is the word used in sentences like "the dragon breathes fire".
- [ ] `lands`: rename the four lands and rewrite their one-line descriptions to suit the topic. Keep the same order and look: woods, dry plains, snow and ice, boss lair.
- [ ] `scrollName`: e.g. "Knowledge Scroll", "Lab Notebook" or "Data Card".
- [ ] `mix`: the share of multiple choice, vocabulary and numerical questions. If the topic has few numbers, lower `n`. Set a type to `0` to switch it off.
- [ ] `defaultDifficulty`: which difficulty is selected on the start screen.

### Step 3: Write the question bank
Replace everything between ``const DEFAULT_BANK = ` `` and `` `; `` with the new questions. Keep a copy of the same lines in a separate `.txt` file for teachers. The game ignores lines starting with `#`.

**Formats (one question per line, parts separated by `|`):**

```
Multiple choice:  chapter | difficulty | question | CORRECT answer | wrong 1 | wrong 2 | wrong 3 | explanation
Vocabulary:       V | chapter | difficulty | definition | answer;other accepted answer | explanation
Numerical:        N | chapter | difficulty | question | answer | allowed error | unit | explanation
```

- `chapter` must match an `id` in `GAME.chapters` exactly (e.g. `7`, `M5`, `IQ1`).
- `difficulty` runs from 1 to 4. Levels 1–2 are recall and simple application, level 3 is two-step reasoning, and level 4 is challenge (calculations, data, combining ideas). Legend and Nightmare draw mainly from levels 2–4.
- Put the **correct answer first** in multiple choice. The game shuffles the options.
- Write an **explanation** for every question in one sentence that teaches, not just "the answer is B". Players see it after every answer.

**How many questions to write**
- At least **60–80 multiple choice** per chapter, so a full game rarely repeats. The Year 9 bank has about 57 per chapter.
- **Every key term** as a vocabulary question.
- **20–30 numerical** per chapter where the content allows it.
- A spread across all four difficulty levels, with most at levels 1–2 for younger years and more at 3–4 for seniors.

**Writing rules**
- Make wrong answers plausible: real misconceptions, not jokes. Keep all four options a similar length.
- Use Australian spelling (aluminium, colour, -ise).
- For formulas, use Unicode subscripts and superscripts (H₂O, CO₂, Na⁺, SO₄²⁻, 10⁻⁵).
- **Never use** the `|` character inside a question, the backtick character, or `${` anywhere in the bank. Any of these breaks the file.
- Don't copy questions from the class's assessment tasks or exams.

**Vocabulary questions**
- Use the textbook definition, but if the definition contains the term itself, replace that word with `_____`.
- Capitals, spaces, hyphens and a final "s" are ignored, but spelling must otherwise be exact.
- Add accepted alternatives after `;` where two answers are truly correct (e.g. `anion;negative ion`).
- Players see the first letter and the letter count on Explorer and Adventurer, and only the letter count on Legend and Nightmare.
- Abbreviations work well as reversed questions: "the 3-letter abbreviation for polyvinyl chloride" → `PVC`.

**Numerical questions**
- `answer` is a plain number. Players can type decimals, negatives, commas, or scientific notation (`1.8e-5`, `1.8 x 10^-5`).
- `allowed error` is ± around the answer: `0` for exact recall (years, counts), and a small margin for calculations (e.g. `0.1` for an answer given to 1 decimal place).
- `unit` is shown beside the answer box, so players type only the number. Leave it empty for years and pure numbers.
- Say the expected rounding in the question ("to 1 decimal place", "to 2 significant figures").

### Step 4: Tune the difficulty for the year group (optional)
Each line of `DIFFS` is one difficulty level. The settings that matter most:

- `hp`: starting health
- `speedMul`: creature speed
- `items`: share of supplies kept on the map
- `extra`: extra creatures per land
- `escape`: correct answers in a row needed to escape
- `qUp`: how much harder the questions get
- `poison`: chance a supply is poisoned
- `bossHp`: boss health
- `window`: seconds the boss's shield stays down

Start from the Year 9 values and change one thing at a time.

### Step 5: Test before giving it to students
- [ ] Open the file in a browser. The start screen should show the new title, year label and chapter buttons, and the line under the buttons should report the expected question counts.
- [ ] If the counts look low, open **Question bank (teachers)** and click **Save and use**. It lists any lines with format errors by line number.
- [ ] Play one land. Check at least one of each question type, a fight, a supply and a scroll, then press R to see the report card.
- [ ] Check the end screen (pause, then "End game and see results").
- [ ] Try it on a school laptop.

### Step 6: Share it
- Give students the HTML file (it works offline), or publish it as a link from a Claude project and share the link.
- Teachers can edit questions inside the game (**Question bank (teachers)**), but those edits save only in that browser. To change questions for everyone, edit `DEFAULT_BANK` in the file and share the new file.

---

## 4. Years 7 and 8 (previous chapter + current chapter)

Set up exactly like Year 9, with **two chapters**. For example:

```js
chapters: [
  { id: '5', label: 'Chapter 5', menu: 'Chapter 5: <previous topic>' },
  { id: '6', label: 'Chapter 6', menu: 'Chapter 6: <current topic>' }
],
```

Other things to change for Stage 4:
- **Reading level:** keep questions to one or two short sentences. Avoid double negatives and "Which is NOT…" questions at difficulty 1–2.
- **Difficulty spread:** about 40% level 1, 40% level 2, 15% level 3 and 5% level 4.
- **Default difficulty:** `defaultDifficulty: 0` (Explorer) for Year 7. Year 8 can stay on Adventurer.
- **Question mix:** typed answers are harder for younger students. Try `mix: { mc: 0.7, v: 0.2, n: 0.1 }` for Year 7 and `{ mc: 0.65, v: 0.2, n: 0.15 }` for Year 8.
- **Vocabulary:** keep to the textbook's key terms and simple definitions. Spaces, hyphens, capitals and a final "s" are already ignored, so "food chain", "Food-chain" and "food chains" all match. Add alternatives after `;` only for genuinely different correct words.
- **Numerical:** mostly recall and one-step calculations (reading a value, simple percentages, unit conversions). Set `allowed error` generously.
- **Creatures and lands:** rename them to suit the topics. For a cells and body systems topic, try "Germ Goblin", "Rogue Cell" (purple blob), "Super Virus", "Raptor", "Fog Phantom" and "Bacteria Dragon". For forces and energy, try "Friction Goblin", "Wobbly Atom", "Static Spark", "Speed Raptor", "Drag Wraith" and "Gravity Dragon".
- **Poison:** consider lowering `poison` on Explorer to `0.1` for Year 7, so the surprise doesn't feel unfair to younger players.

## 5. Year 12 (Module 5 only)

**First confirm the subject**, because Module 5 is different in each course:

- Chemistry: Equilibrium and Acid Reactions
- Biology: Heredity
- Physics: Advanced Mechanics
- Earth and Environmental Science, or Investigating Science: check the course

**Chapters:** use one entry for the whole module.

```js
chapters: [ { id: 'M5', label: 'Module 5', menu: 'Module 5' } ],
```

With one entry the chapter chooser hides itself. For a more useful report card, you can instead list each **inquiry question** as its own "chapter" (e.g. `IQ1`–`IQ4`, with `label: 'IQ1: Static and dynamic equilibrium'`). The report card then shows results per inquiry question.

Other things to change for Stage 6:
- **Match the syllabus language:** use NESA verbs and the module's content dot points. Tag each question to an inquiry question if you use the IQ option above.
- **Difficulty spread:** about 15% level 1, 35% level 2, 30% level 3 and 20% level 4. Use level 4 for multi-step, HSC-style calculations and data interpretation.
- **Question mix:** seniors need calculation practice. Try `mix: { mc: 0.5, v: 0.15, n: 0.35 }`.
- **Default difficulty:** Adventurer or Legend.
- **Numerical questions:**
  - Chemistry: K_eq, pH and pOH, [H⁺], Ka, titration and stoichiometry. Players can type `1.8e-5` or `1.8 x 10^-5`.
  - Physics: projectile range and time, circular motion, orbital speed and period.
  - Biology: probabilities from Punnett squares, allele frequencies.
  - Say the significant figures in each question and set `allowed error` to match (about half of the last digit).
- **Vocabulary:** the module's glossary terms (e.g. Chemistry: dynamic equilibrium, Le Chatelier's principle, conjugate base, buffer, titration, equivalence point).
- **Multiple choice:** HSC style, with four options built on common misconceptions. Don't reuse questions from past HSC papers or school trials that students will sit.
- **Tone:** creature and land names can be wittier for seniors, e.g. "Le Chatelier's Goblin", "Unbalanced Equation" (purple blob), "Strong Acid Virus", "Raptor of Rate", "Fog of Uncertainty" and "Equilibrium Dragon".

---

## 6. Ready-to-paste prompt for Claude in another project

> I've uploaded the Atoms & Earth Quest game (HTML), its Year 9 question bank, and the tailoring guide. Please make a version for **[Year 7 / Year 8 / Year 12 Module 5 – subject]** using **[the chapter files / notes I've uploaded]**. Follow the tailoring guide: update the GAME settings (title, year label, chapters, creature and land names, question mix, default difficulty), write a new question bank in the three formats (multiple choice, vocabulary for every key term, numerical), keep it separate from our assessment tasks, test the game in a browser, and give me the finished HTML plus the question bank as a .txt file.
