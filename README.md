# Blue Blood Basketball: Do Elite College Programs Produce Elite NBA Players?
 
A statistical investigation into whether NBA players drafted from "Blue Blood" college basketball programs — Duke, Kentucky, Kansas, UNC, and UCLA — outperform their peers at the professional level, using exploratory analysis, advanced analytics, linear regression, and hypothesis testing.
 
## Overview
 
In the modern "one-and-done" era of college basketball, Blue Blood programs have become widely regarded as NBA pipelines. This project tests whether that reputation is statistically warranted. Using NBA draft data from 2000–2016, players from Blue Blood schools are compared against all other draftees across basic box score stats, advanced analytics, and career win shares — culminating in a formal hypothesis test to determine whether the performance gap is statistically significant.
 
**Research Question:** Do NBA players drafted from Blue Blood schools between 2000 and 2016 perform better at the professional level than other players drafted alongside them?
 
**Hypothesis:** Players from Blue Blood schools have performed better in the NBA than other players who entered the league during the same timeframe.
 
## Dataset
 
NBA draft data sourced year-by-year from [Basketball Reference](https://www.basketball-reference.com/draft/), covering all drafted players from **2000 to 2016**. Individual yearly files were downloaded and combined into a single data frame.
 
- **2000** was chosen as the start of the modern era
- **2016** was chosen as the cutoff to ensure every player had at least 5 years of career data
- **Undrafted players are not included**
**Key variables:**
 
| Variable | Description |
|----------|-------------|
| `College` | University attended |
| `Year` | Draft year |
| `Pk` | Draft pick number |
| `MPG` | Minutes per game |
| `PPG` | Points per game |
| `APG` | Assists per game |
| `RPG` | Rebounds per game |
| `WS` | Career win shares |
| `BPM` | Box plus minus |
| `VORP` | Value over replacement player |
| `Blue_Blood` | Derived variable — `"Yes"` if college is Duke, Kentucky, Kansas, UNC, or UCLA |
 
Missing values for all numeric performance statistics are imputed as 0, reflecting players who never recorded meaningful playing time.
 
## Methods
 
### Exploratory Analysis
- Draft representation: number of players drafted per school, proportion of total draftees from Blue Blood programs
- Per game statistics: mean MPG, PPG, APG, RPG by Blue Blood status
- Advanced analytics: VORP, BPM, and Win Shares per player by university (filtered to schools with ≥ 10 players drafted to avoid small-sample distortion)
- Win shares over time: scatter plot with smoothed trend lines by Blue Blood status and draft year
### Linear Regression
Two nested linear models predicting career win shares:
1. `WS ~ Year` — draft year only
2. `WS ~ Year + Blue_Blood` — draft year plus Blue Blood status
R² and adjusted R² are compared to assess whether Blue Blood status explains additional variance beyond draft year alone.
 
### Hypothesis Test
A permutation-based two-sample hypothesis test (10,000 simulations) evaluates whether the observed difference in mean win shares between Blue Blood and non-Blue Blood players could arise by chance under the null hypothesis of independence.
 
$$H_0: \mu_{BB} = \mu_{N} \quad \text{vs.} \quad H_a: \mu_{BB} > \mu_{N}$$
 
## Results
 
### Draft Representation
The five Blue Blood schools collectively account for **12.76%** of all draftees from 2000–2016 while occupying all five top spots in total players drafted among universities — a significant over-representation relative to the hundreds of other programs.
 
### Per Game Statistics
 
| Metric | Blue Blood | Non-Blue Blood |
|--------|-----------|----------------|
| MPG | ~20 min | ~15 min |
| PPG | +43% higher | — |
| APG | +65% higher | — |
| RPG | +34% higher | — |
 
Blue Blood players play roughly 42% of a game on average vs. 31% for other draftees, indicating that coaches trust and rely on them more.
 
### Advanced Analytics
Filtering to schools with ≥ 10 players drafted:
- **VORP per player:** UCLA, Kentucky, and Duke appear in the top 10; all five Blue Bloods produce players with positive VORP (above the average bench player threshold)
- **BPM per player:** UCLA, Kentucky, Duke, and UNC appear in the top 10; all five Blue Bloods exceed the −2 threshold considered to have a real negative impact
- **Win Shares per player:** Four of five Blue Bloods appear in the top 10; Kansas is the lone exception at 15.13 WS per player
### Linear Regression
 
| Model | R² | Adj. R² |
|-------|----|---------|
| `WS ~ Year` | 0.0179 | 0.0168 |
| `WS ~ Year + Blue_Blood` | 0.0285 | 0.0266 |
 
Adding Blue Blood status improves both R² and adjusted R², confirming it contributes explanatory power beyond draft year alone. The regression coefficient for Blue Blood status is **+7.80 win shares** — holding draft year constant, a Blue Blood player is expected to accumulate nearly 8 more career win shares than a comparable non-Blue Blood draftee.
 
### Hypothesis Test
 
The observed difference in mean win shares between Blue Blood and non-Blue Blood players yielded a **p-value of 0.0045**. At a significance level of α = 0.05, the null hypothesis is rejected — there is sufficient evidence that Blue Blood players have a statistically significantly higher mean career win shares total.
 
## Key Findings
 
- Blue Blood programs are over-represented in the draft relative to their count, and their players outperform peers across every metric examined.
- The performance advantage holds across basic stats (PPG, RPG, APG, MPG), advanced efficiency stats (BPM, VORP), and career impact (Win Shares).
- Linear regression confirms Blue Blood status adds predictive value for career win shares even after controlling for draft year.
- A permutation hypothesis test confirms the win shares advantage is statistically significant (p = 0.0045).
- Draft pick distributions show Blue Blood players are concentrated in the lottery and top-20 selections, consistent with scouts independently valuing them highly.
## Limitations
 
- **Undrafted players are excluded** — this omits players from both groups and could affect the magnitude of the observed differences, though the direction is unlikely to change given that most undrafted players have limited NBA success.
- **Win Shares as a catch-all metric** — while it captures efficiency, volume, and longevity, no single statistic fully encapsulates player value. The consistency of results across multiple metrics mitigates this concern.
- **Recent draft classes have fewer accumulated win shares** — draft year is included as a covariate in the regression to account for this truncation effect.
## Tech Stack
 
- R
- `tidyverse` (data wrangling and visualization)
- `tidymodels` (linear regression)
- `infer` (permutation hypothesis test)
- `patchwork` (multi-panel plot layout)
- `ggridges` (draft pick distribution visualization)
- `knitr` (report generation)
## Repository Structure
 
```
Blue-Blood-Basketball/
├── data/
│   └── 2000.csv              # Combined NBA draft data (2000–2016)
├── written-report.Rmd        # Full analysis report (R Markdown)
├── written-report.pdf        # Rendered report
├── Blue Blood Basketball PPT.pdf  # Presentation slides
├── USCLAP.Rmd                # Supplementary analysis
├── USCLAP.pdf
├── proposal.Rmd              # Original project proposal
├── proposal.pdf
└── project.Rproj             # RStudio project file
```
 
## How to Run
 
```r
install.packages(c("tidyverse", "tidymodels", "patchwork", "ggridges", "knitr"))
```
 
Open `project.Rproj` in RStudio and knit `written-report.Rmd` to reproduce the full analysis. The data file `data/2000.csv` must be present in the working directory.
 
## Authors
 
Matthew O'Donnell, Gian Castillero, Kevin Kavilaveettil, Eric Zhang
 
