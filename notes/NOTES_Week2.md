# CS5702 Modern Data — Week 2 Notes (Detailed)

> Covers **Lecture 2: The Richness of Data** (31 slides).
> Source: `CS5702_W2_RichData.pdf`. Lab 2 material to be added when available.
> All R code below was executed and verified — outputs are real, not assumed.

---

## The one-sentence summary

Week 1 asked *"what is data science?"*. Week 2 answers *"where does data come from, what forms does it take, how do I get it into R correctly, and what are R's limits?"* — the practical groundwork before any analysis. It maps directly to **LO1** (understand modern data; identify methods for management, retrieval, sharing).

---

## 1. The 4 V's of Modern Data — the headline framework

Modern data differs from traditional data on four dimensions. This is the most quotable, most exam-friendly content in the lecture.

| V | What it means | Example from slides | The challenge it creates |
|---|---------------|---------------------|--------------------------|
| **Volume** | Sheer scale — petabytes, zettabytes | Social media → terabytes of content daily | Won't fit in memory; needs sampling or special tools |
| **Variety** | Many forms & formats | One site: transactions + reviews + images + logs | Hard to combine; no single schema |
| **Velocity** | Speed — real-time, streaming | Markets in milliseconds; IoT non-stop | Can't just "download once"; needs pipelines |
| **Veracity** | Quality & trustworthiness | Social media is noisy but useful once cleaned | Must clean & validate before trusting results |

**Deeper point:** the slides stress data is transformative *"not just because of size — it's variety, speed and connections."* Volume alone isn't what's new; it's all four together, plus the richness of *connections* between datasets.

**How to use it in the exam/coursework:** when handed ANY dataset, run it through the 4 V's as a checklist:
- *Volume* — is it too big for R's RAM? (R rule: ~2× RAM needed, see §6)
- *Variety* — is it one clean format or a mix I must reconcile?
- *Velocity* — static snapshot or a live feed that changes?
- *Veracity* — can I trust it? What's noisy, missing, or biased?

That framing alone earns marks in a report's "data description" section.

> Some sources add more V's (Value, Variability) — but Brunel's slides use these **four**. Stick to four unless asked to extend.

---

## 2. Data sources — a taxonomy of contrasts

Data comes in opposing pairs. Understand the *distinction* in each pair (exam questions ask you to classify a given dataset).

| Contrast | Left side | Right side | How to tell them apart |
|----------|-----------|------------|------------------------|
| **Structured ↔ Unstructured** | Databases, spreadsheets (fixed rows/cols) | Social media posts, emails, images | Does it fit neatly in a table? |
| **Temporal ↔ Spatial** | Stock prices over time, log files | GIS data, maps, floor plans | Is the key dimension *time* or *location*? |
| **Human ↔ Machine** | Surveys, interviews | Sensor readings, IoT | Who/what generated it? |
| **Static ↔ Streaming** | Historical datasets (download once) | Live feeds (constantly updating) | Does it change after you get it? |
| **Public ↔ Private** | Government open data | Business internal data | Who can access it? |
| **Domain-specific** | Medical scans, DNA, research papers, CCTV, reviews | — | Specialised format/field |

**Semi-structured** is a useful middle category worth knowing (not explicit on the slide but exam-relevant): JSON, XML, CSV-with-irregularities — has *some* structure (tags/keys) but not a rigid table. CSV sits at the structured end; a tweet with hashtags and metadata is semi-structured.

> **Golden age of text:** *"From tweets to research papers, text is everywhere and we now have the tools to analyse it."* Text is unstructured but hugely valuable — this is the teaser for **Weeks 8–9** (NLP, word clouds, sentiment analysis).

---

## 3. Data types in R — connected to statistical scales

This is where **CS5702 (data)** meets **CS5701 (stats)**. Stevens' (1946) scales of measurement map onto R types. **This is the highest-value exam concept in Week 2** because "what's wrong with this analysis?" questions usually hide a type/scale mismatch.

| Scale | Example | R type | What operations make sense |
|-------|---------|--------|----------------------------|
| **Nominal** | Country, Sex | `factor` (restricted set) or `character` (free text) | Count/mode only — NO mean |
| **Binary** | Yes/No, T/F | `logical` (special nominal) | Count; coerces to 0/1 in maths |
| **Ordinal** | Likert scale 1–5 | ordered `factor`, or `numeric` if treated as ordered | Order/median — mean is debatable |
| **Interval** | Temperature °C | `numeric` | +/−, mean (no true zero → no ratios) |
| **Ratio** | Length, Weight | `numeric` | All arithmetic (has true zero) |
| **Absolute** | Counts | `integer` (use `L` suffix) | Counting |

### Why the mapping matters — the Gender trap (verified)

If you code a category as numbers, R will do nonsensical maths without complaint:

```r
gender_num <- c(1, 2, 1, 2, 2)     # 1 = female, 2 = male
mean(gender_num)
# [1] 1.6   <-- MEANINGLESS. "Average gender" is nonsense.
```

The fix — make it a **factor**:

```r
gender_fac <- as.factor(c("F","M","F","M","M"))
levels(gender_fac)   # "F" "M"
table(gender_fac)
# gender_fac
# F M
# 2 3            <-- counts, which IS meaningful
```

This is exactly the slide 17 warning: *"Be careful about numeric values/levels for factors (e.g., Gender). Why?"* — because R treats numbers as numbers unless you convert them.

### What a factor really is (verified)

A factor looks like text but is stored as **integer codes + a lookup table of levels**:

```r
f <- factor(c("low","high","med","low"), levels = c("low","med","high"))
f
# [1] low  high med  low
# Levels: low med high
as.integer(f)
# [1] 1 3 2 1    <-- stored as integer codes under the hood
```

Setting `levels` in the right order makes it an **ordered** categorical — essential for Likert/ordinal data so "low < med < high" is respected.

### integer vs numeric — the `L` suffix (verified)

```r
is.integer(10L)    # TRUE   — the L forces integer
is.integer(10)     # FALSE  — plain 10 is numeric (floating point)!
is.integer(10.1)   # FALSE  — numeric
class(10L)         # "integer"
class(10)          # "numeric"
```
**Surprise:** `10` without the `L` is NOT an integer in R — it's a double/float. Only `10L` is a true integer.

### The type-coercion gotcha (verified)

```r
x <- c(T, T, T, F, F)
min(x)    # 0      (not FALSE)
sum(x)    # 3      (counts the TRUEs!)
mean(x)   # 0.6    (proportion of TRUEs — actually useful)
```
R silently coerces `logical → 0/1` the moment you do arithmetic. `sum()` on a logical vector = "how many TRUE", and `mean()` = "proportion TRUE" — a genuinely handy trick, once you expect it.

### Coercion hierarchy inside c() (verified)

A vector must be ONE type, so `c()` coerces everything to the most general type present:

```r
c(1, "two", TRUE)   # "1" "two" "TRUE"   <- all become CHARACTER (most general)
c(1, TRUE, 2L)      # 1 1 2             <- logical+int+numeric become NUMERIC
```
**Hierarchy (narrow → wide):** `logical → integer → numeric → character`. Mix any, and R promotes to the widest. This is *why* vectors can't hold mixed types (a Week 1 quiz point).

**Rule for life:** after loading data, ALWAYS run `str(df)` or `class(col)` to check every column got the type you expect.

---

## 4. Data frames — the deeper view

| Property | Detail |
|----------|--------|
| What it is | A collection of **aligned vectors** (columns), each possibly a different type |
| Shape | 2-dimensional, **rectangular** (all columns same length) |
| vs matrix | A matrix is all ONE type; a data frame mixes types |
| Editing | Can't edit cells directly like Excel — you modify via **code** |
| Why efficient | Each column is a vector → simple, fast, vectorised operations |
| Inspect | `head()`, `str()`, `summary()`, `View()`, `nrow()`, `ncol()` |

### `str()` is your best friend (verified)

```r
df <- data.frame(name = c("A","B"),
                 age  = c(30L, 25L),
                 paid = c(TRUE, FALSE),
                 stringsAsFactors = FALSE)
str(df)
# 'data.frame': 2 obs. of 3 variables:
#  $ name: chr  "A" "B"
#  $ age : int  30 25
#  $ paid: logi  TRUE FALSE
```
`str()` shows, per column: the **type** (`chr`, `int`, `logi`, `num`, `Factor`) and the first few values. Make this the FIRST thing you run on any new data frame.

---

## 5. Reading and writing files — the core practical skill

The most coursework-relevant part of Week 2. Your analysis always *starts* with loading a file.

### Reading a CSV
```r
# Store a long URL in a variable first for readability (slide 19 tip)
fname <- "https://raw.githubusercontent.com/mjshepperd/CS5702-Data/master/pubs.csv"

pubsDataFrame <- read.csv(fname,
                          header = TRUE,            # row 1 holds column names
                          stringsAsFactors = FALSE,  # keep text as character, not factor
                          encoding = "UTF-8")        # handle accents/symbols correctly
head(pubsDataFrame)
```

| Argument | What it does | Recommended |
|----------|--------------|-------------|
| `header` | Is the first row column names? | `TRUE` (almost always) |
| `stringsAsFactors` | Auto-convert text columns to factors? | `FALSE` (modern default; convert deliberately when you want) |
| `encoding` / `fileEncoding` | Character set | `"UTF-8"` — avoids garbled special characters |
| `sep` | Column separator | default `","`; use `sep="\t"` for TSV, `";"` for European CSV |
| `na.strings` | What counts as missing | e.g. `na.strings = c("", "NA", "NULL")` |

**File-picker trick (slide 22):**
```r
myDF <- read.csv(file.choose())   # pops up a dialog to locate the file
```
Great for quick local work; avoid in final scripts (not reproducible — hard-code the path/URL instead).

### Writing a CSV
```r
write.csv(myDF, "mydata.csv", row.names = FALSE)
```
- `row.names = FALSE` → don't dump R's row numbers as a phantom first column (a very common beginner mistake that corrupts the next read).
- By default `write.csv` **overwrites** silently — be careful.

**CSV round-trip caveat (verified):** types mostly survive (`chr`, `int`, `logi` came back intact in testing), but CSV stores plain text — on re-reading, R *re-guesses* each column's type. Numbers that were integers may come back as numeric, dates come back as text, etc. **Always `str()` after re-reading.**

### Encoding — why UTF-8 (slide 21)
UTF-8 supports all characters (accents, non-English scripts, symbols) and works across platforms. Using it avoids the classic "£" → "Â£" or "é" → "Ã©" corruption. Rule: use `UTF-8` on both import and export.

---

## 6. Databases and R (awareness level)

| Point | Detail |
|-------|--------|
| Reality | Data often lives in **databases** (SQLite, MySQL, Oracle), not loose CSVs |
| Database = | A collection of **tables**, not a single file |
| Goal | Join/organise tables into ONE rectangular data frame for analysis |
| Why use one | For very large data — query only what you need, don't load it all |
| R connection | Packages `DBI` + a driver like `RSQLite`; you can **embed SQL** in R code |

**For your coursework:** you'll almost certainly use CSV, not a database. But if an exam asks *"ways to get data into R"*, the four answers are: **CSV/flat files, built-in datasets, APIs/web, and databases via DBI.** (SQL is also the #2 in-demand skill from the Week 1 lab — worth knowing it exists.)

---

## 7. R and large data — limits & solutions (exam awareness)

The "Big Data" reality: terabytes → petabytes → exabytes → **zettabytes** (global datasphere ~180 ZB by 2025), driven by IoT, AI, cloud, streaming.

### R's restrictions
1. **In-memory** — R holds everything in RAM (typical laptop 8–32 GB).
2. **~2× RAM rule** — you need roughly double the raw data size, because manipulations create **copies**.
3. **Slow import** — reading very large files takes longer than the analysis itself.
4. **~2 billion element** limit per vector (≈ 2×10⁹).

### Solutions (if asked)
1. **Specialised packages** (often C++ backed): `{data.table}`, `{bigmemory}`, `{ff}` for out-of-memory access.
2. **Scale up / out:** parallelisation, cloud compute, Hadoop via `{RHadoop}`.
3. **But in this module:** *"we focus on small data (in-memory) where R excels."* So don't over-engineer — your datasets are small.

---

## 8. Exam relevance — ranked

| Concept | Exam value | Why / likely question |
|---------|-----------|-----------------------|
| **4 V's** | ⭐⭐⭐ HIGH | "Describe the challenges of this dataset" → frame with Volume/Variety/Velocity/Veracity |
| **Structured vs Unstructured** (+ semi-structured) | ⭐⭐⭐ HIGH | "Classify these data sources" — direct LO1 |
| **Scale → R type mapping** | ⭐⭐⭐ HIGH | "What's wrong with this analysis?" → wrong type (mean of a category) |
| **Type coercion gotchas** | ⭐⭐ MEDIUM | "What does `min(c(T,T,F))` output?" / `c(1,"a",T)` type |
| **integer vs numeric (`L`)** | ⭐⭐ MEDIUM | "Is `10` an integer in R?" → No, it's numeric |
| **R memory limits / big data** | ⭐⭐ MEDIUM | "What are R's limitations with large data?" |
| **`read.csv()` parameters** | ⭐ LOW exam / ⭐⭐⭐ coursework | Not tested directly, but step 1 of CS5801 |
| **Databases/DBI** | ⭐ LOW | Awareness only; naming data sources |

---

## 9. Mini self-test (try before the lab — answers below)

1. A dataset of live tweets with hashtags and timestamps — classify it on 3 of the contrast pairs.
2. What does `sum(c(TRUE, FALSE, TRUE, TRUE))` return, and why?
3. Survey codes Gender as 1/2 and Satisfaction as a 1–5 Likert. What R type should each be, and what's the danger of leaving them numeric?
4. Is `100` an integer in R? How do you force an integer?
5. Why use `row.names = FALSE` when writing a CSV?

<details>
<summary>Answers</summary>

1. **Unstructured** (free text + metadata, really semi-structured), **Streaming** (live feed), **Human-generated**, **Temporal** (timestamps), **Public**. Any 3 valid.
2. **3** — logicals coerce to 1/0, and `sum` counts the TRUEs.
3. Gender → **factor** (nominal); Satisfaction → **ordered factor** (ordinal). Danger: left numeric, R computes meaningless means like `mean(Gender)=1.6`, and treats Likert gaps as equal intervals.
4. **No** — `100` is numeric (float). Use `100L` (or `as.integer(100)`).
5. So R's auto row-numbers aren't written as a phantom first column that corrupts the next read.
</details>

---

## 10. Week 2 goals checklist (from slide 29)

- [ ] Appreciate the richness of data — list characteristics (4 V's) and types (the contrast pairs)
- [ ] Be familiar with major sources of free/public data
- [ ] Use R to create and manipulate **vector** and **data frame** variables
- [ ] Use R to **import and export CSV** files (`read.csv` / `write.csv`)
- [ ] Complete the Week 2 worksheet and quiz
