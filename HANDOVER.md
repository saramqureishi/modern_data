# HANDOVER — CS5702 Modern Data study sessions

> **For the next AI assistant:** read this whole file before replying to Sara. It explains who she is, how she wants to be taught, what's been covered so far, and what comes next. Follow the working style in Section 2 exactly.

---

## 1. Who the student is and what she's aiming for

- **Student:** Sara. Postgraduate at **Brunel University of London**, MSc programme (Data Science & Analytics / AI pathway), academic year **2026/27, Semester 1**.
- **Module:** **CS5702 Modern Data** (study block). Module leader: Prof Martin Shepperd. Co-leader: Dr Ferdoos Hossein Nezhad. Lecturer: Dr Valeriia Haberland.
- **Sister module:** **CS5701 Quantitative Data Analysis** (Dr Isabel Sassoon). It runs in parallel and **shares the same assessments**.
- **Tooling:** R + RStudio on **Windows**, with RMarkdown (`.Rmd`). She had **never used R or RStudio before Week 1**. She now runs chunks, knits to HTML and works with data frames confidently.
- **The framing she asked for:** *"You are a teacher with only 200 hours with me. Your only objective is to make me functional before time runs out. No theory without practical use, no generic lists."*

### Assessment structure (important)
CS5701 and CS5702 are two teaching blocks that **share ONE pair of assessments**:

| Code | Type | Notes |
|------|------|-------|
| **CS5801** Quantitative Data Analysis | **Coursework**: an authentic, real-world data analysis project in R/RMarkdown | Submitted via **WISEflow**. Deadlines are **11:00 UK time**. Up to 48h late is penalised; more than 48h late = NS (non-submission). |
| **CS5802** Critical Analysis of Modern Data | **Exam**: **2 hours, closed book** | Critique and understanding count for more than syntax. |

- **Pass (C- or above) requires meeting the LOs.** CS5702: **LO1** understand modern data and identify methods for management, retrieval and sharing. **LO2** critically evaluate data quality and plan preparation.
- ❗ **Still missing:** the actual **CS5801 assessment brief** (task, rubric, deadline). Neither study guide has it. Ask Sara for it; once you have it, align all teaching to its rubric.
- The **AI-use policy** allows LLMs but warns they can be wrong. Students are expected to **test and verify** generated code. Model this behaviour: run code to verify it, and don't just assert answers.

---

## 2. How Sara wants to work (FOLLOW THIS)

1. **Practical-first, exam-aware.** Every topic should end with: *what's examinable* (closed-book CS5802) and *what's needed for the coursework* (CS5801). Use ⭐ ratings (⭐⭐⭐ HIGH / ⭐⭐ MED / ⭐ LOW) for exam relevance.
2. **Weekly rhythm.** She shares each week's **lecture slides (PDF)** and **lab (`.Rmd`)** via GitHub. For each week:
   - **Lecture notes:** *very detailed, don't miss anything* (her words). Cover every slide's content, and explain the reasoning as well as the facts. Include: a one-sentence summary, the key concepts as tables, verified R examples, a ranked exam-relevance table, exam one-liners she can reuse, and a short self-test with answers.
   - **Lab:** she asked to do it **"in one go instead of slow one by one"** from Week 3 onwards. Give a **complete walkthrough in one response**: every exercise, its answer, the reasoning, and clearly flagged **deliberate bugs** with their fixes. (Weeks 1–2 were step-by-step because she was new. Don't go back to that unless she asks.)
3. **Verify everything by running it.** Run the R code yourself where you can, and say in the notes that outputs were verified. Never present unverified numbers as fact.
4. **Tell her what to skip.** For example: knitting to PDF/LaTeX (always knit to **HTML**), pie charts, memorising base-R graphics parameters, and the internals of autoencoders/PCA.
5. **Be direct and honest.** If something is missing or you made a mistake, say so plainly. (Example: in the Week 1 quiz, the correct answer to "what is a package" was *"an external set of functions"*, and the assistant had initially said otherwise.)
6. **Troubleshooting style.** When she pastes an R error, give the exact fix, explain the error in one line, and list what to check if it still fails. Keep it short.
7. **Notes live in this repo** as Markdown files under `notes/`. Save each new week as `notes/NOTES_WeekN.md` in the same format.
8. She **can't easily attach large files** to chat. Large PDFs and Word files arrive as unreadable binary. **Use this GitHub repo** as the way she shares files: download from `raw.githubusercontent.com` and extract PDF text (for example with Python `pypdf`).

---

## 3. Repo map (`saramqureishi/modern_data`)

| File | What it is | Status |
|------|-----------|--------|
| `CS5702_StudyGuide_2026-27_v2.pdf` | Module study guide | ✅ Covered |
| `CS5702_W1_Intro.pdf` / `CS5702_W1_Lab.*` | Week 1 lecture + lab | ✅ Done |
| `CS5702_W2_RichData.pdf` | Week 2 lecture | ✅ Notes done |
| `CS5702_W2_Lab (1).Rmd` | Week 2 lab (module survey analysis, word clouds) | ⏳ **NOT done yet** |
| `CS5702_W3_EngineerOrHack.pdf` / `CS5702_W3_Lab.Rmd` | Week 3 lecture + lab | ✅ Done |
| `Week4_CS5702-Lecture4.pdf` / `Week4_CS5702_Lab4.Rmd` | Week 4 lecture + lab (EDA & visualisation) | ⏳ **NEXT. Not started** |
| `CS5702_Understanding_ggplot2.Rmd` | ggplot2 + plotly helper | ⏳ Not reviewed |
| `notes/NOTES_Week1.md`, `NOTES_Week2.md`, `NOTES_Week3.md` | Her revision notes | ✅ Written |

The CS5701 study guide was in her older repo `saramqureishi/modern_data_notes`. It isn't needed again.

---

## 4. Progress log: what's been covered

### Week 1 (Intro + Lab 1): ✅ complete
- Did the lab step by step in RStudio. Learned: running chunks, **knitting to HTML** (hit the `pdflatex not found` error; fixed by setting `output: html_document` / Knit → HTML), variables, types, vectors, data frames (`$`, `[row,col]`, `nrow`/`ncol`), `read.csv`.
- Verified answers: 3.1 = 2400; 3.2 gross 2400 / net 1920; 3.5 = 32×11; 3.6 mean mpg 20.09 / min 10.4.
- Week 1 quiz done (10 Qs). The answer key is in `notes/NOTES_Week1.md`.
- **Exam-relevant points from Week 1:** the LLM "should have challenged the prompt" critique (the skewness demo), why pie charts are bad, the definition of data science, and why R.

### Week 2 (Richness of Data): ✅ lecture notes / ⏳ lab not done
- Covered: the 4 V's, the data-source contrast pairs, Stevens' scales → R types, factors, coercion, `read.csv`/`write.csv` arguments, databases/DBI, R's memory limits.

### Week 3 (Engineering or Hacking?): ✅ lecture notes + full lab walkthrough
- Covered: the NHS/PHE Excel `.xls` disaster, reproducibility vs replicability, FAIR, metadata, Knuth's literate programming, `install.packages()` vs `library()`, user-defined functions.
- Lab verified against the live OWID COVID data: 617,667 rows × 61 columns → 10 columns → 2,434 UK rows; max UK new cases in one day = 275,647; all 4 debugging bugs fixed (`varI`, reversed `<`/`>`, letter `O` vs zero, character coercion); functions `third`, `fraction`, `notNegative` (plus a defensive version).

### Recent troubleshooting
- `there is no package called 'validate'` and `'Hmisc'`: fixed with `install.packages("validate")` / `install.packages("Hmisc")` run once in the Console, then `library()`. These packages are **not** used in any lab file in this repo (Weeks 1–4). They probably come from a CS5701 lab or the upcoming Week 5 (data quality/cleaning) material. Confirm with her.

---

## 5. Next steps (in order)

1. **Week 4:** EDA & visualisation (lecturer FHN). Detailed lecture notes from `Week4_CS5702-Lecture4.pdf` + a one-go lab walkthrough of `Week4_CS5702_Lab4.Rmd` (salary & gender analysis, ggplot2). This is one of the **highest-value weeks** for marks. Save as `notes/NOTES_Week4.md`.
2. **Week 2 lab** (`CS5702_W2_Lab (1).Rmd`): still owed. It needs `tidyverse`, `wordcloud`, `RColorBrewer`, `wordcloud2` and `tm`.
3. **Get the CS5801 coursework brief** from Sara and map the teaching to its rubric.
4. Week 5 (data quality, cleaning, imputation) is the other **highest-value week**. Expect packages like `validate`/`Hmisc`.

### The "one exercise" recommended to her (not done yet; remind her)
Write **one page** critiquing the Week 1 "top-10 data science tools" dataset and its pie chart. Cover: source bias (job adverts ≠ actual demand), percentages that overlap and don't sum to 100, US-only and 2025-only sampling, and what you'd need to trust it. This trains the critique skill the CS5802 exam rewards.

---

## 6. Module schedule (for planning)

| Wk | Lecture | Lab |
|----|---------|-----|
| 1 | R ecosystem | Basic R ✅ |
| 2 | Structured/unstructured data | Module survey ⏳ |
| 3 | Engineering vs hacking | Importing data, time series, smoothing ✅ |
| 4 | **EDA & visualisation** | Salary & gender analysis ⏳ NEXT |
| 5 | **Data quality, cleaning, imputation** | Cleaning + imputation |
| 6 | Reading week | — |
| 7 | Presenting data | GenAI in your workflow |
| 8 | NLP & text processing | Text mining, word clouds |
| 9 | Sentiment analysis | Sentiment in R |
| 10 | Dimensionality reduction | PCA & autoencoders |
| 11 | Exam (CS5802) revision | Coursework support |

Core reading: Kabacoff *R in Action*; Peng & Matsui *The Art of Data Science*; Wickham *R for Data Science* (r4ds.hadley.nz); the online "Modern Data" book (bookdown.org/martin_shepperd/ModernDataBook).
