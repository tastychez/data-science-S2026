The Islands, Part 1: Design
================
Hong Zhang
2026-10-29

- [Grading Rubric](#grading-rubric)
  - [Individual](#individual)
  - [Submission](#submission)
- [Setup](#setup)
  - [**q1** Orientation](#q1-orientation)
  - [**q2** Occupied homes](#q2-occupied-homes)
  - [**q3** Find common names: Sequential
    sample](#q3-find-common-names-sequential-sample)
  - [**q4** Plan a random sample](#q4-plan-a-random-sample)
  - [**q5** Collect the random sample](#q5-collect-the-random-sample)
  - [**q6** Find common names: Random
    sample](#q6-find-common-names-random-sample)
  - [**q7** Write a helper function](#q7-write-a-helper-function)
  - [**q8** Construct a bootstrap confidence
    interval](#q8-construct-a-bootstrap-confidence-interval)
  - [**q9** Discover possible
    measurements](#q9-discover-possible-measurements)
  - [**q10** Planning a study
    (TEAMWORK)](#q10-planning-a-study-teamwork)

*Purpose*: So far in this class, I’ve handed you each dataset. But much
of the important work of statistics happens *before* data collection.
Issues with data collection generally can’t be fixed by fancy analysis
after the fact. Therefore, in this *two part* challenge, you will first
*plan* and then *execute* a statistical project.

To do this, we’ll make use of The Islands, an extremely detailed
simulation of a virtual population, developed by researchers at the
University of Queensland. This is a fascinating sandbox that helps
illustrate the difficulties and complexities of collecting real data,
while still being more tractable than collecting data from real humans.

This is part 1 of 2. In this part you will *plan* your statistical
project, particularly your data collection. Sound data collection is
called *statistical design*.

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

# Setup

<!-- ----------------------------------------------------------------------- -->

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rsample)

filename_random <- "./data/helvig-random.csv"
```

### **q1** Orientation

Log into [The Islands](https://islands.smp.uq.edu.au/index.php) and head
to the [Visitor Center](https://islands.smp.uq.edu.au/visitors.php) on
Providence island (to the east). Watch the first three videos on the
`Guides` tab.

*Note:* There is no deliverable for this task. BUT, you’ll need to be
oriented to The Islands in order to do *any* of the tasks below.

The Islands is an *incredibly* detailed simulation of a real population.
Islanders are born, die, and move around. The Islands was designed as a
teaching tool to help students learn how to apply statistics to
collecting data in the real world.

### **q2** Occupied homes

Find the total number of homes in Helvig, and count the number of
unoccupied homes. Answer the questions below.

NB. When Zach counted the homes (on 2025-01-08), he found 20 unoccupied
out of 536 total. That means the occupied percentage was about 96%. But
you still need to count, because your numbers *will* be different!

**Observations**:

- What is the total number of homes in Helvig?
  - 626 homes.
- What is the number of unoccupied homes? (*Hint*: This is not given
  anywhere. You will have to count them!)
  - 0.  I checked the resident list for every numbered home and all 626
        had at least one person living there.
- What percent of homes are *occupied*?
  - 100% (626 of 626).
- Are there any sources of *real* uncertainty in the percent occupied
  you calculated?
  - Yes. The Islands changes over time as people are born, die, and
    move. Zach counted 536 homes with 20 unoccupied on 2025-01-08, which
    is about 96% occupied. My count is a snapshot of one day and could
    be different a week later.
- Are there any sources of *erroneous* uncertainty in the percent
  occupied you calculated?
  - Yes. I could have made a counting mistake, such as skipping a house
    or counting a tree tile or public building as a home.

Zach looked at the first 25 homes in Helvig and recorded the `age` and
`name` of every person in those homes. These people are provided in
`helvig-seq.csv`.

``` r
## NOTE: Do not edit this
df_sample_seq <- read_csv("./data/helvig-seq.csv")
```

    ## Rows: 63 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): name
    ## dbl (2): house, age
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_sample_seq 
```

    ## # A tibble: 63 × 3
    ##    house   age name             
    ##    <dbl> <dbl> <chr>            
    ##  1     1    58 Arvid Thorn      
    ##  2     1    55 Benjamin Connolly
    ##  3     2    46 Remy Tiwari      
    ##  4     2    44 Sophia Solberg   
    ##  5     2    14 Illona Solberg   
    ##  6     2    12 William Tiwari   
    ##  7     2    10 Hans Solberg     
    ##  8     3    24 Kaya Carlsen     
    ##  9     3    23 Julian Eklund    
    ## 10     3     4 Katharina Carlsen
    ## # ℹ 53 more rows

You’ll use this dataset as a starting point to figure out prevalent
*last names* in Helvig.

### **q3** Find common names: Sequential sample

Complete the code below to find the prevalence of the most common *last
name* in Helvig using the sample `df_sample_seq`. Answer the questions
below.

Make sure to include in `df_q3` the counts as a column named `n`, and
the prevalence (number of occurrence divided by total people) as a
column `p`. Also, make sure to sort the data in *descending* order of
prevalence.

*Hint*: You will have to use what you’ve learned about string handling
to extract the *last names* only!

``` r
## TASK: Compute the prevalence and sort
df_q3 <-
  df_sample_seq %>%
  # drop the NA row for the empty house since it is not a person
  filter(!is.na(name)) %>%
  mutate(last = str_extract(name, "\\w+$")) %>%
  count(last) %>%
  mutate(p = n / sum(n)) %>%
  arrange(desc(p))

df_q3
```

    ## # A tibble: 27 × 3
    ##    last         n      p
    ##    <chr>    <int>  <dbl>
    ##  1 Sorensen     8 0.129 
    ##  2 Lund         6 0.0968
    ##  3 Solberg      5 0.0806
    ##  4 Eklund       4 0.0645
    ##  5 Blomgren     3 0.0484
    ##  6 Carlsen      3 0.0484
    ##  7 Jansen       3 0.0484
    ##  8 Morris       3 0.0484
    ##  9 Banerjee     2 0.0323
    ## 10 Chunduri     2 0.0323
    ## # ℹ 17 more rows

Use the following to check your work.

``` r
## NOTE: No need to change this
## Check that data has `p` column and is in descending order
assertthat::assert_that(
              all(df_q3 %>%
                 mutate(d = p - lead(p)) %>% 
                 filter(!is.na(d)) %>% 
                 pull(d) >= 0
              )
            )
```

    ## [1] TRUE

``` r
print("Very good!")
```

    ## [1] "Very good!"

*Observations*

- What last name is most prevalent in `df_sample_seq`?
  - Sorensen. The q3 table shows 8 Sorensens, which is a prevalence of
    0.129.
- Is this sample representative of *all* houses in Helvig? Why or why
  not?
  - No. It only covers the first 25 houses on the map, which are next to
    each other in one part of Helvig. People in the same house or the
    same family often share a last name. A few families in those 25
    houses can make their names look much more common than they are in
    all of Helvig.

In the exercises, we talked about the importance of random sampling. In
the previous challenge, we were able to *simulate* a random sample by
running a few lines of code. But in the real world, we have to work
harder to gather a random sample. We’ll do this in two stages: *plan*
then *collect*.

### **q4** Plan a random sample

Complete the code below to draw a sample of size `n=25`. Replace
`n_houses` with the (current) total number in Helvig.

``` r
## TASK: Set the parameters for this code block

## Select a random sample of houses
n_houses <- 626 # Total number of houses
n_sample <- 25 # Desired sample size

set.seed(101)   # Set a seed for reproducibility

df_numbers_random <- 
  tibble(
    house = sample(
        1:n_houses,     # All integers from 1 to n_houses
        n_sample,       # Size of our sample
        replace = FALSE # Sample *WITHOUT* replacement
      )
  ) %>% 
  # Arrange for our data collection convenience
  arrange(house)

# Pull the column so we can list just the house numbers
df_numbers_random %>% 
  pull(house)
```

    ##  [1]  14  95  97 117 131 204 209 240 244 246 315 316 351 352 355 430 442 454 474
    ## [20] 483 504 521 557 571 625

Use the following code to check your results.

``` r
## NOTE: No need to change this
assertthat::assert_that(
  all(dim(df_numbers_random) == c(25, 1))
)
```

    ## [1] TRUE

### **q5** Collect the random sample

Gather the names of all people in the homes you listed in the previous
exercise. Match the same columns as `df_sample_seq`; those are, `house`,
`age`, `name`. Make sure to include `NA` rows for houses with no
occupants. Save your data as a CSV with the filename provided in the
variable `filename_random`. Answer the questions below.

``` r
## NOTE: Do not edit
filename_random
```

    ## [1] "./data/helvig-random.csv"

Note that this points to the `data/` subdirectory in your `challenges`
folder.

The following code will load your data.

``` r
## NOTE: Do not edit
df_sample_random <- 
  read_csv(filename_random)
```

    ## Rows: 67 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): name
    ## dbl (2): house, age
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Use the following to check your work.

``` r
## NOTE: No need to change this
# Check that the dataset has the correct column names
assertthat::assert_that(setequal(
  df_sample_random %>% names(),
  df_sample_seq %>% names()
))
```

    ## [1] TRUE

``` r
# Check that all of the house numbers in the dataset match those that were planned
numVsamp <- 
  anti_join(
    df_numbers_random,
    df_sample_random %>% distinct(house),
    by = "house"
  ) %>% 
  pull(house)
assertthat::assert_that(
  length(numVsamp) == 0,
  msg = str_c("You are missing the houses: ", numVsamp)
)
```

    ## [1] TRUE

``` r
sampVnum <- 
  anti_join(
    df_sample_random %>% distinct(house),
    df_numbers_random,
    by = "house"
  ) %>% 
  pull(house)
assertthat::assert_that(
  length(sampVnum) == 0,
  msg = str_c("You have extra houses: ", sampVnum)
)
```

    ## [1] TRUE

``` r
print("Great work!")
```

    ## [1] "Great work!"

*Observations*

- Which sample—sequential or random—is more *representative* of all
  homes Helvig? Why?
  - Random.
  - `sample()` gave every house from 1 to 626 the same chance of being
    picked. The houses in my random sample are spread from 14 to 625.
    The sequential sample only has houses 1 to 25. It can only tell us
    about that one part of the village.

### **q6** Find common names: Random sample

Run the code below to find the prevalence of the most common *last name*
in Helvig using the sample `df_sample_random`. Answer the questions
below.

``` r
# NOTE: No need to edit; run and answer the questions below
df_sample_random %>% 
  mutate(last = str_extract(name, "\\w+$")) %>% 
  count(last) %>% 
  arrange(desc(n)) %>% 
  mutate(p = n / sum(n))
```

    ## # A tibble: 28 × 3
    ##    last         n      p
    ##    <chr>    <int>  <dbl>
    ##  1 Sato         9 0.134 
    ##  2 Collins      6 0.0896
    ##  3 Jensen       5 0.0746
    ##  4 Blomgren     4 0.0597
    ##  5 Wilson       4 0.0597
    ##  6 Carlsen      3 0.0448
    ##  7 Edwards      3 0.0448
    ##  8 Hall         3 0.0448
    ##  9 Kimura       3 0.0448
    ## 10 Varma        3 0.0448
    ## # ℹ 18 more rows

*Observations*

- Did you find any highly prevalent names using `df_sample_random` that
  you *didn’t* find in q3 (using `df_sample_seq`)? Write them here.
  - Yes. Sato is the most common name in the random sample with 9 people
    (p = 0.134). Collins (6), Jensen (5), and Wilson (4) are next. None
    of these four are in the top rows of the q3 table. Sorensen, the top
    name in q3, is not in the top rows of the q6 table.
- Is there any reason that people with the same last name might tend to
  *live near each other*?
  - Yes. People in the same household often share a last name since
    children usually get a parent’s last name. Relatives may also live
    close to each other. A sample of houses that are next to each other
    is likely to pick up several members of the same family.

You should have found some difference between the sequential and random
samples. This is because we’re only working with a *sample*—a limited
number of observations from the entire population. We could go to every
single house in Helvig and determine *exactly* how many people of each
name there are (that’s called a *census*), but the point of statistical
inference is that we can make statements about a population using only a
sample. What makes statistical inference powerful is that we can
determine *how confident* we should be in our results, based on the size
of our sample.

To do this, we’ll use the bootstrap approach that you saw in the
exercise sequence. We’ll start by building a helper function.

### **q7** Write a helper function

Complete the code below to write a helper function. Your function will
compute the proportion of people in a sample that have a user-specified
last name.

``` r
## TASK: Write a helper function that takes a dataframe with full names 
#  (provided in a `name` column), removes any invalid rows, and computes the
#  proportion of individuals with the user-specified `last` name (returned
#  in an `estimate` column).
name_prevalence <- function(df, last = "Collins") {
  df %>%
    # rows with no name are empty houses and not people
    filter(!is.na(name)) %>%
    summarize(estimate = mean(str_detect(name, last))) %>%
    mutate(term = "prevalence")
}
```

Use the following code to check your results.

``` r
## NOTE: No need to change this
# Find the most prevalent name in the data
last_most <- 
  df_sample_random %>% 
  mutate(last = str_extract(name, "\\w+$")) %>% 
  count(last) %>% 
  arrange(desc(n)) %>% 
  slice(1) %>% 
  pull(last)

# Ensure correct columns
assertthat::assert_that(
  setequal(
    tibble(name = c("James")) %>% name_prevalence(., last = "James") %>% names(),
    c("term", "estimate")
  ),
  msg = "Your code should result a dataframe with just two columns: `term` and `estimate`"
)
```

    ## [1] TRUE

``` r
# Ensure NA handling
assertthat::assert_that(
  !(tibble(name = c(NA_character_, "James")) %>% 
    name_prevalence(., last = "James") %>% 
    pull(estimate) %>% 
    is.na()),
  msg = "Ensure your code properly ignores NA's"
)
```

    ## [1] TRUE

``` r
# Check for correctness
assertthat::assert_that(
  name_prevalence(df_sample_random, last = last_most) %>% pull(estimate) ==
    mean(str_detect(df_sample_random$name, last_most), na.rm = TRUE),
  msg = "Your code computed the wrong value"
)
```

    ## [1] TRUE

``` r
print("Nice!")
```

    ## [1] "Nice!"

### **q8** Construct a bootstrap confidence interval

Choose a prevalent name that you found in q6. Use bootstrap resampling
with your helper function to construct a confidence interval for the
prevalence of that name. Answer the questions below.

*Hint*: We learned how to do resampling-based inference in `e-stat09`.

``` r
# TASK: Complete the code below to compute a bootstrap-based confidence interval
# set a seed so the bootstrap resamples are the same every time
set.seed(101)
df_interval_bootstrap <-
  df_sample_random %>%
  bootstraps(., times = 1000) %>%
  mutate(
    estimate = map(
      splits,
      function(split_df) {
        analysis(split_df) %>%
          name_prevalence(last = "Sato")
      }
    )
  ) %>%
  ## NOTE: No need to edit this line; this uses your bootstrap sample to compute
  # a confidence `int`erval using the percentile method
  int_pctl(., estimate)

df_interval_bootstrap 
```

    ## # A tibble: 1 × 6
    ##   term       .lower .estimate .upper .alpha .method   
    ##   <chr>       <dbl>     <dbl>  <dbl>  <dbl> <chr>     
    ## 1 prevalence 0.0597     0.135  0.224   0.05 percentile

**Observations**:

- What is highest possible prevalence for your chosen name, based on the
  confidence interval you constructed?
  - I chose Sato, the most common name in my random sample. The q8 table
    shows a 95% interval from 0.0597 to 0.224. The highest plausible
    prevalence of Sato in Helvig is about 0.224, or about 22% of people.
  - This interval is likely too narrow. The bootstrap resamples people
    one at a time. I sampled whole houses and people in the same house
    often share a last name. Resampling houses instead of people would
    give a better sense of the uncertainty.
- Note that we used the *random* sample with the bootstrap procedure in
  this task. Could we use the bootstrap to make a confidence interval
  using the sequential sample (`df_sample_seq`) that would be
  representative of all of Helvig? Why or why not?
  - No.
  - The bootstrap only resamples the people who are already in the
    sample. It shows how much an estimate would change by chance. It
    can’t fix a sample that only covers houses 1 to 25. A bootstrap
    interval from `df_sample_seq` would describe those 25 houses and not
    all of Helvig. For example, it would center on Sorensen being 0.129
    of people, even though Sorensen is not in the top rows of the q6
    table for the random sample.

### **q9** Discover possible measurements

Click on a single islander (you can do this from one of the houses), and
take a look at what info they can provide. Write down three pieces of
info that you’re particularly interested in; this will inform your
quantity of interest for the final task.

*Hint*: The videos from the [Visitor
Center](https://islands.smp.uq.edu.au/visitors.php) will be especially
helpful for getting some ideas.

- Arm curl test score, which comes from the arm curl test on the Tasks
  tab.
- Income, which is listed on each islander’s About page.
- Age and sex, which are also on the About page and could affect arm
  strength.

### **q10** Planning a study (TEAMWORK)

Challenge 10 will be a team assignment where you plan and execute a
statistical study using The Islands. You should meet with your learning
team for Challenge 10 to complete the steps below—though everyone is
responsible for including the plan in their individual Challenge 08
submission.

#### Population

- Residents of Bjurholm. We will record each islander’s age as a
  covariate since it could affect arm strength.

#### Quantity of interest

- How much a monetary incentive changes the number of arm curls an
  islander can do. Each islander does the arm curl test before and
  after. That way each person is compared to themselves. We will measure
  the effect as the average before to after change for the incentive
  group minus the average before to after change for the control group.

#### Covariates

- Income, from each islander’s About page. A cash incentive might matter
  more to someone with a low income than to someone who is already rich.
- Age and sex, since both are likely related to arm strength.
- Job, since some jobs are more physical than others.

#### Observation or experiment?

The Islands allows you to ask islanders to complete tasks. If you just
take measurements on your participants, then it’s an *observational
study*. But if you also introduce something that’s meant to change the
outcome of a measurement (e.g., drinking coffee before taking a test),
that’s called an *experimental study*. You need to decide whether your
study is observational or experimental.

- Experiment. We are giving some islanders a monetary incentive to see
  if it changes their arm curl performance.

#### Question / Hypothesis

- What effect does offering a monetary incentive have on arm curl
  performance for residents of Bjurholm?
  - Our hypothesis is that islanders who are offered money will improve
    more from before to after than islanders who are not offered money.
  - We also expect the effect to be smaller for islanders with higher
    incomes, since the same amount of money means less to them.

#### Sampling plan

- Steps to collect the data
  1.  List every occupied house in Bjurholm by its house number.
  2.  Use `set.seed()` and `sample()` to pick houses at random. In each
      chosen house, pick one resident at random.
  3.  Ask the chosen islander for consent. If they decline, record their
      age, sex, and income from their About page and draw a new house.
      This lets us check whether the people who declined are different
      from the people who took part.
  4.  Use `sample()` to randomly assign each islander who consents to
      either the incentive group or the control group, with the same
      number in each group.
  5.  Before (everyone): have the islander do the arm curl test 3 times
      and record all 3 scores. We will use the average of the 3 as their
      before score.
  6.  After: the incentive group is offered the same amount of money
      before each of 3 more arm curl tests. The control group does 3
      more arm curl tests with no money offered. Record all 3 scores and
      average them as the after score.
  7.  Record income, age, sex, and job from each islander’s About page.
  8.  Every team member follows these same steps and uses the same
      incentive amount every time. That way no one makes decisions on
      the fly that could bias the results.
- How will you ensure the data is representative of your chosen
  population?
  - Houses are picked at random with `sample()` instead of by
    convenience, like walking down one street. We will also compare the
    people who declined with the people who took part, using their age,
    sex, and income, to check whether the people who agreed are
    different from everyone else.
- For experiments only: How will you ensure any effects you observe are
  due to the treatment, and not due to other factors?
  - Each islander is measured before and after. This cancels out
    differences between people, like some islanders just being stronger.
    The control group does the same before and after tests but gets no
    money. If islanders improve just from practice, or get worse from
    being tired, the control group will show that too. Comparing the
    change in the incentive group with the change in the control group
    removes those effects. Randomly assigning islanders to the two
    groups means things like income, age, and fitness should be spread
    evenly between the groups, the same way randomizing the plots spread
    out the effect of the river in the randomization exercise.
- How will you choose your sample size?
  - We are aiming for 30 islanders in each group (60 total). When we
    tried asking islanders for consent, 5 of 13 declined (about 38%).
    That means we expect to approach about 100 islanders to get 60. Each
    test takes real time in The Islands. This is about what our team can
    collect in a reasonable time. After the first 10 islanders in each
    group, we will compute a bootstrap confidence interval for the
    effect. If it is too wide to tell whether the incentive helps, we
    will add more islanders if time allows.
