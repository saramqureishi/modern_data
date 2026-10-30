# CS5702 Modern Data — Week 3 Notes (Detailed)

> Covers **Lecture 3: Engineering or Hacking?** (32 slides) + **Week 3 Lab**.
> Source: `CS5702_W3_EngineerOrHack.pdf`, `CS5702_W3_Lab.Rmd`.
> All R below was executed and verified against the LIVE OWID COVID dataset (617,667 rows).

---

## The one-sentence summary

Week 3 is the module's **philosophy week**: the difference between *hacking* (code that happens to work) and *engineering* (code that is understandable, reproducible, and trustworthy). It maps to **LO2** (critically evaluate data and plan preparation) and introduces **user-defined functions**. Theme from Lecture 1 returns: *"a disciplined engineering approach, not hacking."*

---

## 1. Why does it matter? — the motivating examples

### The unreadable code puzzle (slides 5–8)
Martin shows deliberately awful code:
```r
g2 <- as.integer(readline(prompt="Enter g2: "))
if(g2<1){paste("Error")}else{g1<-1;
for(g3 in 1:g2) {g1 <- g1*g3}; paste("g1 is",g1)   }
```
Nobody can tell what it does at a glance. It computes a **factorial**. The lesson progression:
1. **Rewrite for humans** — meaningful names (`n` not `g2`), indentation, comments.
2. **Then realise** — R already has `factorial()` built in! The whole loop collapses to:
   ```r
   n <- as.integer(readline(prompt = "Enter number:"))
   paste("Factorial", n, "is", factorial(n))
   ```
**Two lessons:** (1) write code humans can read; (2) before writing anything, check if a built-in function already exists.

### The NHS / PHE COVID disaster (slides 9–12) — the headline cautionary tale
Real event, 2020. Testing labs sent daily results as CSV → merged into **Excel using the old `.xls` format (from 1986)**. The `.xls` format has a hard limit of **~65,000 rows**. Each test used multiple rows, so each file maxed out at **~1,400 cases**. Once exceeded, **further cases were silently dropped** — ~16,000 COVID cases lost, contact tracing missed.
> **The lesson:** using the wrong tool / an obsolete format, with no validation, had real public-health consequences. This is "hacking" with real-world cost. **Prime exam material** for "why does disciplined data engineering matter?"

### To err is human (slide 13)
Shepperd's own research: audited 49 ML papers (2,456 results) → **22 of 49 contained demonstrable errors** (transcription, calculation, statistical). Errors are common and often hard to detect → hence the need for reproducibility.

---

## 2. Make it understandable and reproducible

### Literate programming (slide 14)
> *"Instead of imagining that our main task is to instruct a computer what to do, let us concentrate rather on explaining to human beings what we want the computer to do."* — **Donald Knuth, 1984**

This is *why* RMarkdown exists — code + prose explanation together. Your notebooks should explain **how AND why**.

### FAIR data principles (slide 16) — memorise the acronym
Data reusable by others should be:
| Letter | Principle | Meaning |
|--------|-----------|---------|
| **F** | Findability | Can others locate it? (identifiers, metadata) |
| **A** | Accessibility | Can they retrieve it? (open, documented access) |
| **I** | Interoperability | Does it work with other data/tools? (standard formats) |
| **R** | Reusability | Clear licence + metadata so it can be reused |

### The "mystery dataset" problem (slide 17)
A dataset with columns like `1/2`, `score`, `03/04/26`, `ID` raises questions:
- What do `1` and `2` mean? (codes with no key)
- What does `score` measure — units? range?
- `03/04/26` = 3 April or 4 March? (date ambiguity — **always use ISO `YYYY-MM-DD`**)
- What does `ID` identify? What population?
→ **Avoid mysteries** by providing **metadata**.

### Metadata (slide 18) = "data about data"
Describes how the dataset is organised and what each variable means. Example: `PersonID` is alphanumeric (string), must be unique and exactly 8 characters. Metadata makes meaningful processing possible.

### Reproducibility vs Replicability (slide 19) — know the difference (exam!)
| Term | Definition |
|------|------------|
| **Reproducibility** | Same data + same code → **same results** (others can repeat YOUR analysis) |
| **Replicability** | **New data**, same study → **similar findings** |
> *"Minimally, we need reproducibility in data science."* Reproducibility is the floor; replicability is the stronger scientific goal.

---

## 3. Write code for humans

### Readable code tips (slide 21)
- **Names** — meaningful, descriptive (`checkInput` not `ci`). The single biggest factor in code literacy.
- **Comments** — explain *why*, not *what*. (And keep them updated — "obsolete comments are extremely harmful.")
- **Layout** — indentation and spacing.
- **Prettyprinting** — consistent formatting.
- **User-defined functions** — the big one (section below).

### User-defined functions (slides 22–24)
**Benefits:**
- **Abstraction** — hide messy detail behind a clear name (`checkInput()`)
- **Locality** — related code in one place
- **Reuse** — write once, call many times
- **Maintainability** — fix bugs in ONE place

**Anatomy of a function:**
```r
halve <- function(n) {        # 1. NAME (halve) + 2. ARGUMENT(S) (n)
  functionResult <- n / 2     # 3. BODY (what it does)
  return(functionResult)      # 4. RETURN VALUE (what comes back)
}
halve(-8.3)   # -> -4.15  (verified)
```
| Part | Role |
|------|------|
| Function name | How you call it, e.g. `head()` |
| Argument(s) | Values passed in; 0 or more |
| Body | Statements defining behaviour (hidden for built-ins) |
| Return value | The result handed back to the caller |

---

## 4. Manage the whole analysis

### Why be disciplined? (slide 26)
- **Multiple versions** — which is the "right" file?
- **Dependencies** — package/data versions
- **Backup** — don't lose work
- **Wasted time** — redoing lost steps
- **Avoid careless mistakes**

### Packages & libraries (slides 27–28) — the install/load distinction (exam favourite)
- **Base R** = what you get automatically.
- **Packages** = community extensions, validated by **CRAN** (~20,000–25,000 of them), sometimes GitHub.

```r
install.packages("tidyverse")   # ONCE — downloads to disk. QUOTES required.
library(tidyverse)              # EVERY session — loads into RAM. NO quotes.
```
| Function | When | Quotes? | Analogy |
|----------|------|---------|---------|
| `install.packages("x")` | Once (download) | **Yes** | Buying a book for your shelf |
| `library(x)` | Every session (load) | **No** | Taking the book off the shelf to read |

### Final discipline tips (slide 29)
1. Keep **raw data read-only** (never overwrite the original)
2. Manipulate data **reproducibly** (in code, not by hand)
3. Use **RStudio Projects**
4. Consider **GitHub** (version control)

---

## 5. Week 3 goals (slide 30)
- [ ] Appreciate the need for **engineered** data analysis
- [ ] Know tactics to improve **understandability/quality**
- [ ] Be able to implement **simple user-defined R functions**

---

## 6. Exam relevance — ranked

| Concept | Exam value | Likely question |
|---------|-----------|-----------------|
| **NHS/Excel `.xls` disaster** | ⭐⭐⭐ HIGH | "Why does disciplined data engineering matter? Give an example." |
| **Reproducibility vs Replicability** | ⭐⭐⭐ HIGH | "Define and distinguish..." |
| **FAIR principles** | ⭐⭐⭐ HIGH | "What makes data reusable?" |
| **install vs library** | ⭐⭐⭐ HIGH | "Difference between the two? Why quotes on one?" |
| **Metadata / mystery data** | ⭐⭐ MEDIUM | "What is metadata and why does it matter?" |
| **Function anatomy** | ⭐⭐ MEDIUM | "Name the parts of a function" / write one |
| **Literate programming (Knuth)** | ⭐⭐ MEDIUM | "Why write code for humans?" |
| **Readable-code tips** | ⭐ LOW/MED | List factors affecting code literacy |

---

## 7. Key exam one-liners (steal these)

- *"Hacking produces code that happens to work; engineering produces code that is understandable, reproducible and maintainable."*
- *"Reproducibility = same data + same code → same result; replicability = new data → similar findings. We minimally need reproducibility."*
- *"The PHE case shows an obsolete tool (`.xls`, 65K-row limit) silently dropped ~16,000 COVID cases — a hacking failure with real consequences."*
- *"`install.packages()` downloads once (quotes needed); `library()` loads every session (no quotes)."*
- *"FAIR = Findable, Accessible, Interoperable, Reusable."*
