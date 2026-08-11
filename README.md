# Data Processing & Visualization with BASH and R — Large-Scale Twitter Dataset

This project processes and analyses a large-scale COVID-19 Twitter dataset using the BASH shell and R. It covers navigating and decompressing large files, extracting and filtering data at scale with core Unix text-processing tools, and visualising engagement patterns in R.

---

## Dataset

- **corona_tweets.csv.gz** — 118 MB compressed, tab-separated
- **1,143,559 tweets** (1,143,558 excluding header)
- **641,975 unique Twitter users**
- 13 columns: Created, Tweet_ID, Text, User_ID, User, User_Location, Followers_Count, Friends_Count, Geo, Place_Type, Place_Name, Place_Country, Language

---

## Part 1: Shell Navigation & File Inspection

- Copied and inspected the compressed dataset (`cp`, `ls -lh`)
- Decompressed and previewed the file header using `gunzip -c` piped into `head -1`
- Counted total records using `gunzip -c | wc -l`

---

## Part 2: Text Filtering & Pattern Matching

- Counted unique Twitter users by extracting the User_ID column, sorting, and de-duplicating (`awk -F'\t' '{print $4}' | sort | uniq | wc -l`)
- Counted tweets mentioning "vaccine" (case-insensitive, whole-word match) using `grep -iwc`: **16,569 tweets**
- Isolated tweets using non-standard capitalisation of "vaccine" (e.g. "VACCINE", "VaCcInE") by excluding standard-case matches: **270 tweets**
- Exported filtered results to file using output redirection (`>`)

---

## Part 3: Follower Distribution Analysis

Segmented all 641,975 unique users into 10 follower-count brackets using `awk` range filters, `cut`, `sort`, and `uniq`:

| Follower Range | Unique Users |
|---|---|
| ≤ 1,500 | 498,480 |
| 1,501 – 2,500 | 43,891 |
| 2,501 – 3,500 | 23,620 |
| 3,501 – 4,500 | 15,165 |
| 4,501 – 5,500 | 9,297 |
| 5,501 – 6,500 | 6,848 |
| 6,501 – 7,500 | 5,076 |
| 7,501 – 8,500 | 3,855 |
| 8,501 – 9,500 | 3,072 |
| > 9,500 | 32,772 |

Results were exported to CSV and visualised in R as a bar chart, confirming the dataset is heavily skewed toward low-follower accounts.

---

## Part 4: Retweet vs. Non-Retweet Engagement

- Filtered out retweets using a regex anchor (`$3 !~ /^RT @/`) and re-compressed the filtered dataset with `gzip`
- Repeated the follower-distribution breakdown on the retweet-only subset across the same 10 brackets, then merged both distributions in R (`merge()`) to produce a side-by-side comparative bar chart

**Finding:** Across every follower-count bracket, retweets consistently outnumbered original (non-retweet) posts, and the gap was most pronounced in the lowest follower bracket (≤1,500 followers). This indicates that lower-follower accounts on Twitter engaged with COVID-19 content primarily by retweeting rather than posting original content, and that retweet activity dominates the platform's engagement pattern regardless of follower count.

---

## Tools & Techniques

- **BASH:** `gunzip`, `awk`, `grep`, `sort`, `uniq`, `cut`, `wc`, pipes, output redirection
- **R (RStudio):** `read.table()`, `barplot()`, `merge()`, `png()` device export

---

## Files

- `corona_tweets.csv.gz` — raw compressed dataset
- `No_retweets_file2.csv.gz` — retweet-filtered dataset
- `Result.txt` — non-standard-case "vaccine" mention export
- `datascience6.csv`, `datascience8.csv` — follower distribution summaries (all tweets vs. retweet-filtered)
- `Twitter_users_according_to_their_followers_barchart1.png` — follower distribution chart
- `Retweet_comparsion_Noretweet2.png` — retweet vs. non-retweet comparative chart

---

## Author

Nashmia Shakeel
