# CS5702 Modern Data — Week 1 Notes

> Covers **Lecture 1 (Introduction)** + **Week 1 Lab (Basic R)**.
> Source: `CS5702_W1_Intro.pdf`, `CS5702_W1_Lab.pdf/.Rmd`. All code answers below were run and verified in R.

---

## 0. How this module is assessed (read once, remember forever)

- **CS5702 Modern Data** (this module) + **CS5701 Quantitative Data Analysis** are TWO teaching modules that **share ONE pair of assessments**:
  - **CS5801 — Quantitative Data Analysis = COURSEWORK** (an authentic, real-world data analysis project; submitted via WISEflow; deadlines 11:00 UK time; 48h late = capped, >48h = NS).
  - **CS5802 — Critical Analysis of Modern Data = EXAM** (2 hours, **closed book**).
- Modern Data supplies the *data* skills (sources, quality, cleaning, EDA, visualisation, NLP); QDA supplies the *statistics*. Both assessments test the combination.
- **Pass = C- or above by meeting the Learning Outcomes:**
  - **LO1:** understand modern data; identify methods/tech for management, retrieval, sharing.
  - **LO2:** critically evaluate data quality; plan preparation/cleaning.
- STILL TO GET: the actual **CS5801 assessment brief** (task + rubric + deadline) — it's on Brightspace/WISEflow, revealed in Lecture 1.

---

## 1. Lecture 1 — concepts (examinable)

- **Definition to memorise:** data science turns *raw data into understanding, insight, and knowledge* (Wickham & Grolemund).
- **Why now:** more + richer data + more powerful tools → opportunities to collect, clean, merge, analyse, visualise — "for good, and sadly for less good purposes" (ethics hook).
- **Why R:** designed by statisticians; strong data wrangling/storage; flexible graphics; integrates with ML (TensorFlow) and C/C++/Python; large open community.
- **GenAI message (the key Lecture 1 idea):** ChatGPT produced a "negatively skewed" distribution that wasn't actually skewed (skewness ≈ 0.065). The real failure = **it should have challenged the prompt.** "LLM tools are clever but not magic — you still need to learn R." → Always RUN and sanity-check generated code.

---

## 2. Week 1 Lab — mechanics (where coursework marks are built)

### RMarkdown
- `.Rmd` = text (markdown) + live R code chunks → **Knit to HTML** (ignore PDF/LaTeX — it breaks lab machines).
- Three parts: (1) YAML header between `---`, (2) markdown text, (3) code chunks (```` ```{r} ... ``` ````).
- Insert chunk: `Cmd/Ctrl+Alt+I`. Run chunk: green triangle.

### R essentials
- Assign with `<-`. R is **case-sensitive** (`price` ≠ `Price`). `#` = comment.
- Everything is an **object**; functions are objects too (`mean` vs `mean(x)`).
- Help: `?functionName` e.g. `?head`.

### Data types
| Type | Example | Note |
|------|---------|------|
| numeric | `10.3` | default for numbers |
| integer | `10L` | the `L` suffix forces integer |
| character | `"hi"` | text, in quotes |
| logical | `TRUE`/`FALSE` | `T`/`F` shorthand; coerces to 1/0 in maths |

### Vectors
```r
greetings <- c("Hi!", "Hello", "Good morning")  # same type only
greetings[2]      # 2nd element -> "Hello"
greetings[1:3]    # range
```

### Data frames (THE key structure)
- "A collection of aligned vectors (columns)" — like a spreadsheet.
```r
df <- mtcars
head(df)          # first 6 rows
nrow(df); ncol(df); str(df)   # inspect
df$cyl            # a column by name
df[2, ]           # row 2, all columns
df[2, 1:3]        # row 2, columns 1-3
df["Valiant", ]   # row by name
df["Valiant","cyl"] <- 12     # update a cell
```
- Read external data: `read.csv("https://.../worms.csv")`.

---

## 3. Worked answers (all run & verified in R)

| Ex | Answer | How |
|----|--------|-----|
| 3.1 | **2400** | `200 * (1+0.2) * 10` |
| 3.2 | gross **2400**, net **1920** | `taxRate <- 0.20; net <- gross*(1-taxRate)` |
| 3.4 | `is.vector` TRUE, class **numeric** | a single value is a length-1 vector |
| 3.5 | **32 rows, 11 cols** | `nrow(df)`, `ncol(df)` |
| 3.6 | mean **20.09**, min **10.4** | `mean(df$mpg)`, `min(df$mpg)` |
| 3.7 | **6** | `df["Mazda RX4","gear"] <- 6` |
| 3.8 | mpg 21 / cyl 6 / disp 160 | `df[2, 1:3]` |
| 3.9 | **12** | `df["Valiant","cyl"] <- 12` |

**Pie vs bar (Ex 2.1–2.4):** pie charts are bad because (a) the skill percentages don't sum to 100 (each advert lists many skills), so a pie implies a false "share of a whole"; (b) humans judge angles/areas poorly vs bar heights. Bar chart is better; add value labels so exact percentages are readable.

---

## 4. Week 1 Quiz — answers with reasoning

These aren't for marks — they're self-checks that mirror the exam style (CS5802 closed-book). Every wrong option is a real student mistake.

| # | Question | ✅ Answer | Why |
|---|----------|-----------|-----|
| 1 | How to make a comment? | Start with `#` | `#` tells R to ignore the rest of the line |
| 2 | Vectors can mix types? | **False** | Vectors = same type only. Mixed → use list or data frame |
| 3 | A data frame is? | 2-D, possibly different types | Unlike a matrix (one type) and is always rectangular |
| 4 | `x<-5; X<-x+x; print(x)` | **5** | Case-sensitive: `x` and `X` are different variables |
| 5 | `y<-5; y<-(15/y)*y` | **15** | `(15/5)*5 = 3*5 = 15`; right side evaluates fully, then assigns |
| 6 | What is a package? | **An external set of functions** | Downloaded from CRAN/GitHub; extends base R |
| 7 | Illegal variable name? | **var-1** | `-` is the minus operator; legal: letters, numbers, `.`, `_` |
| 8 | Execute R code directly? | **Console** pane | Bottom-left; Terminal is for OS shell, not R |
| 9 | NOT true of R Projects? | Must be on GitHub | GitHub is optional; `.Rproj` is local |
| 10 | Why install R AND RStudio? | RStudio is a front-end; needs R | R = engine, RStudio = dashboard on top |

---

## 5. The ONE high-leverage exercise
Write **one page** critiquing the Week 1 "top-10 tools" dataset + chart (Ex 2.1 & 2.3):
- **Source bias:** job adverts ≠ actual skill demand; employers over/under-list.
- **Measure:** percentages are of *postings mentioning the tool* and overlap → don't sum to 100 → pie chart is misleading.
- **Sampling:** US-only, 2025-only, "data science" occupation code only.
- **Trust:** what would you need (methodology, sample size, raw counts) to believe it?
This trains the exact "critical analysis" skill the closed-book exam (CS5802) rewards.
