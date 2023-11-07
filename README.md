# CDS 101 Midterm Exam - Fall 2022

For this midterm project you will be conducting exploratory data analysis on a dataset of polls about the upcoming 2022 US national elections. The exercises in this exam will require you to wrangle, visualize, and interpret data.

You must submit this project (by (1) uploading a PDF to Blackboard, and (2) pushing your Rmd file to GitHub) by the due date for your section - **late submissions will not be accepted**.


## Rules

This project has slightly different rules to the normal weekly assignments, as it is designed to assess what you have learned over the course of the semester so far (think of this as an exam rather than a regular assignment).

* You must answer the exercises on your own.

* You may **not** ask other people for help answering the exercises. This includes other students, as well as people in real life or on the internet.

* Do **not** post questions or code about this project in any of the public Slack channels.

* If you need clarification on the question, *please send a Slack direct message to your instructor*. However, while they will clarify what a question means, they will not help you answer it. For example:

  * Asking your instructor to clarify whether a question is asking for 1 or 2 graphs (for example) *might* be OK if it's unclear from the question's instructions.
  * However, they will not tell you how to plot a particular type of graph.
  * Asking them how to fix a bug/error will probably not get an answer if it's due to a problem in your code that you have learned how to write correctly during the first half of the course.

* However, this exam is both take-home and open-book: you may freely make use of the internet (such as Google), previous assignments, and other course materials to research answers, correct code usage, and fix any problems you encounter.


## About the Dataset

You will be working with a dataset of polls conducted ahead of the upcoming 2022 US elections.

In the run-up to elections we often want to understand how people are going to vote. Both the media and the political candidates are interested in understanding the current opinions of the electorate, and so they conduct polls, in which they reach out to a sample of the total population of voters to ask who they plan to vote for in the upcoming election. Individual polls are often not very accurate (due to error or bias), but when analyzed as a group they can reveal interesting trends.

This dataset of polls was originally compiled by the news website FiveThirtyEight, which is famous for using polls to very accurately predict the outcome of several US elections.

You might be interested in reading FiveThirtyEight's articles on this topic, which can be found here: https://fivethirtyeight.com/politics/

Their original dataset has been lightly preprocessed for this project (to remove unnecessary/redundant columns), and the reduced dataset that you will be using is loaded into the `polls` variable when you run the set-up chunk in the `midterm.Rmd` file.

Each row in the dataset represents the polling percentage for a particular candidate. (I.e. a single poll may have multiple rows, each representing a different candidate that was in that poll.)

The columns in the `polls` dataframe are:


| Column name                           | Description            |
| ------------------------------------- | -----------            |
| `poll_id` | Unique ID for each poll. |
| `pollster` | The organization that conducted the poll. |
| `pollster_rating_name` | Another name for the organization that conducted the poll, corresponding to the grade in the `fte_grade` column. |
| `fte_grade` | A rating of the quality of the poll. |
| `methodology` | The methods used to contact people for the poll (e.g. Online, Live Phone, etc.) |
| `state` | The US state in which the poll was conducted. |
| `start_date` | The date when the poll started. |
| `end_date` | The date when the poll ended. |
| `sponsor_candidate` | The candidate that sponsored this poll (if any). |
| `sponsor_candidate_party` | The political party that sponsored this poll (if any). |
| `sample_size` | The number of people contacted for the poll. |
| `partisan` | If this was a partisan poll. |
| `race_id` | Unique ID for the political race that each row measures. |
| `cycle` | The election cycle. |
| `office_type` | The type of office that this race is for, e.g. "Governor". |
| `seat_number` | An identification number for the seat that this race is for. |
| `seat_name` | The name of this seat, if any. |
| `election_date` | The date when this election will be held. |
| `party` | The party that the candidate belongs to. |
| `candidate_name` | The name of the candidate. |
| `pct` | The polling percentage for this candidate. |

## Exercises

Things to remember:

* **Write all your answers in the `midterm.Rmd` file.**
* Don't forget to run the set-up chunk at the top of the `midterm.Rmd` file to load the dataset.
* All graphs should be created with the `ggplot() + geom_FUNCTION()` syntax, *NOT* the `qplot()` function.
* All graphs should have appropriate titles and axis labels. 
* Remember to *make a new commit at the end of each exercise* (you should end with at least 1 commit per exercise by the end of the exam). You can always make additional commits if you need to edit a previously committed answer.


### Part 1

The exercises in this section are worth 7pts in total (out of 15pts for this project in total).

1. (1pt) Answer the following questions about the dataset (no code required).

    i. What does each row in this dataset represent? (I.e. what is it an observation of?)
    
    ii. Which column contains data on the number of people who were sampled in each poll?
    
    iii. Is this dataset in a *tidy* format already (as specified in the Data Wrangling module of the course)? Why or why not?
  
    Commit your work after finishing this exercise.

2. (1pt) Using the `polls` dataset, create a histogram of the polling percentages for all candidates in all polls (i.e. create a histogram of the `pct` column). Don't forget to add a title and appropriate x-axis label.

    Then write a description of the shape of your historam.
    
    Also, state a hypothesis that explains the *modality* of your histogram. (Hint: think about why there might be clusters of candidates at particular polling percentages. Remember that there are two main political parties in the USA, but there are also others.)
  
    Commit your work after finishing this exercise.

3. (1pt) Test your hypothesis by creating a new histogram of the `pct` column which is faceted over the `party` column. You should add appropriate options to this code chunk to make the faceted sub plots large enough to be visible (i.e. the default graph size is too small, so you need to make it larger).

    Referring to the patterns that you can see in your histograms, describe whether this seems to support or refute your hypothesized reason for the modality you saw in Exercise 2's histogram.
  
    Commit your work after finishing this exercise.

4. (1pt) Use the `summarize()` function to calculate the number of distinct polls in each state, and display a table of the six states with the most polls (in descending order). To do this you will need to chain together a number a functions with pipes:

    * First, use the `group_by()` function to group the `polls` dataframe by state.
    
    * Then use the `summarize()` function to calculate the number of distinct polls in each of those state groups. (The `poll_id` column contains a unique identifier for each poll, so we wish to count up how many unique poll identifiers are in each state with the `n_distinct()` summary function.)
    
    * Then sort the output of `summarize()` in descending order.
    
    * Then display just the first six rows of this sorted table with the `head()` function. (`head()` returns just the first 6 rows of a table.)

    A code template is given below matching these points. However it is recommended that you add this code to your answers one function at a time and make sure the code runs *before* adding in the next function (i.e. don't try to copy and complete the entire template in one go.)
    
    ```r
    ... %>%
      group_by(...) %>%
      summarize(
        n = n_distinct(...)
      ) %>%
      arrange(...) %>%
      head()
    ```
    
    This table should be printed out in your answer file (i.e. do not assign it to a new variable).
    
    (If your code is correct, you will find that the most polled state is Florida, with 57 polls. This will be the first row in your output table.)
  
    Commit your work after finishing this exercise.

5. (1pt) Florida is a heavily polled state because it has historically been a "swing state"" (i.e. it does not consistently vote for the same political party every election), and because there are a lot of elections happening in Florida in 2022.

    One of those elections will determine the next Governor of Florida. The Republican party candidate is Ron DeSantis (the current governor, who is up for re-election). The Democratic candidate is Charlie Crist.
    
    A dataframe of polling numbers for just these two candidates has been created for you and stored in the `fl_gov_polls` variable.
    
    Using this reduced `fl_gov_polls` dataset, `group_by()` the candidate (using the `candidate_name` variable) and then use the `summarize()` function to calculate the mean, median, standard deviation, maximum, and minimum of each candidate's poll percentages (the `pct` column).
    
    This table should be displayed in your answer file. Using the numbers you have calculated, which of the two candidates for Florida governor gets higher percentages in the polls? Justify your answer by referencing some of your calculated summary statistics.
  
    Commit your work after finishing this exercise.
    
6. (1pt) Using the `fl_gov_polls` dataframe, create a graph that shows the distribution of polling percentage (`pct`) for the two candidates. This should be an appropriate graph for showing the distribution of a continuous variable (`pct`) but you will need to modify it in some way it so that you can see the separate distributions for each of the two candidates (i.e. the categorical `candidate_name` column).

    The choice of graph is up to you, as is how you modify it to show the two candidates.
    
    Write an interpretation of any patterns you see in your plot.
  
    Commit your work after finishing this exercise.
    
7. (1pt) Using the `fl_gov_polls` dataframe, create a single scatter plot showing polling percentage vs poll date for each candidate. You should also add smoothed trend lines to this same plot (i.e. a trend line for each candidate).

    Note that:
    * You should plot the `pct` column on the y-axis of this graph, and `end_date` should be on the `x-axis`.
    * You should `color` both the points and the trend lines using the `candidate_name` column (the automatically determined colors are fine, even if they are not the colors typically used to represent the Democratic and Republican parties).
    * The smoothed trend lines should be added with the `geom_smooth()` function (do not add the `method = "lm" argument - we want the trend lines to be curved, not linear).
    * As with all the graphs in this exam, don't forget to add an appropriate title and axis labels.
    
    (Side note: the shaded grey area around each trend line is the "confidence interval". You can ignore these for this exercise. They mean that we are confident that if we repeated these polls, then 95% of the time we should expect the trend line to lie within the shaded grey region.)
    
    Write 1 paragraph description and interpretation of the patterns you see in your scatter plot. In particular, do the candidates seem to be closely tied or switching in popularity over time, or is one candidate consistently more popular that the other? Make sure you justify your answer.
  
    Commit your work after finishing this exercise.


### Part 2: Exploratory Data Analysis

In this part of the project, you will pick your own variables from the `polls` dataset and explore them.

* Pick two columns from the full `polls` dataframe. Think carefully about which columns you are picking and what exactly you want to explore (a change in something over time? The covariation between two non-time variables?). Not all combinations of columns make sense to compare (so if you realize later that the two variables are a poor combination then you should change one or both of them). You can read more about each of the variables in the `About the Dataset` section above.
    
    **State the names of the two columns** you've picked in the "Variables of Interest" section of the `midterm.Rmd` file.
    
    You are also welcome to reduce the dataset down to a subset of the rows (using the `filter()` function) if you wish to focus on one particular state or race (as we did in Exercises 5-7).

* (1pt) Calculate summary statistics for each of your chosen variables using the `summarize()` function.

    * Put your code in the "Summary Statistics" section of the `midterm.Rmd` file.
    * You should have 2 `summarize()` functions, one for each variable.
    * You should calculate the mean, median, minimum, maximum, and standard deviation.
    * If the summary statistic is NA, then this might be because there is missing data in the column. You should ignore missing data when calculating all of these summary statistics, by adding the `na.rm = TRUE` argument to each summary function, e.g. (`mean(some_column, na.rm = TRUE)`).
    * You do *not* need to use the `group_by()` function unless one of your variables is categorical - then you should `group_by()` the categorical variable when summarizing the other variable.
    * You do not need to calculate summary statistics for a variable containing a date, if that was one of the two that you chose.
    
    Once you have finished, commit your work for this step.

* (2pts) Create a separate plot for each of the two variables that shows __the variation within each variable__ (i.e. you should create two graphs, one for each of your variables). 

    Note: you do not need to visualize a variable containing a date if that is one of your two variables - in that case you should make just one plot showing the variation within the other variable.

    Put your code and answers in the "Variation of Each Variable" section of the `midterm.Rmd` file.
    
    There are several plots you can use to plot the distribution of a single variable - you can refer to the [What Graph?][what-graph] guide for ideas. You can use the same graph for each variable, or you can try different types of graph.
    
    Add appropriate axis labels and titles to these graphs.

    Then, use your graphs to describe as much as you can about the distribution of the variable in that plot (e.g. center, shape, etc.).
    
    Once you have finished, commit your work for this step.

* (1pt) Create a plot that shows __the co-variation between your two variables__.

    Again, refer to the the [What Graph?][what-graph] guide if you are stuck on an appropriate type of graph to create, but a scatter plot is usually a good first choice for two continuous variables. Other types of graph might also be appropriate: for example, a line plot is only a good idea of one of the variables (on your x-axis) contains a date.
    
    Remember to add appropriate axis labels and a title to this graph.

    Describe the nature of the relationship between the two variables and any patterns you see (e.g. if you made a scatter plot, is the relationship strong or weak, linear or non-linear, etc.)
    
    Put your code and answers in the "Co-variation Between Variables" section of the `midterm.Rmd` file.
  
    Once you have finished, commit your work for this step.

* (1pt) Write 1-2 paragraphs interpreting any patterns you have found (or the lack of a pattern) in in the previous steps and what these patterns means in the broader context of real-world elections.

    Write your interpretation under the "Interpretation" section of the `midterm.Rmd` file.

    Things to think about (you don't need to answer these questions exactly - use them to as starting points for your answer):
    
    * Have you discovered a pattern in the polls?
    * Has your exploratory data analysis confirmed what you expected to find (and if so, what was that?), or does it show something completely different?
    * What might these patterns mean for the upcoming 2022 U.S. elections (in November)?
  
    Once you have finished, commit your work for this step.


## How to submit

To submit your Midterm Exam, follow the two steps below.
Your project will be graded for credit **after** you've completed both steps!

1.  Save, commit, and push your completed RMarkdown file so that everything is synchronized to GitHub.
    If you do this right, then you will be able to view your completed file on the GitHub website.

2.  Knit your RMarkdown document to the PDF format, export (download) the PDF file from RStudio Server, and then upload it to *Midterm Exam* posting on Blackboard.


## How this Midterm Exam will be graded

This midterm is worth 15 points in total, broken down as:

* 1pt for pushing your final code to GitHub (with at least 1 commit for each exercise, and appropriately descriptive commit messages).
* 1pt for formatting of the PDF that is uploaded to Blackboard (e.g. don't print out tables larger than ~1 page, don't let code and tables overflow the right margin, etc.)
* 1pt for spelling and grammar (all answers should be written in full sentences).
* 12pts for the answers to the exercises above (7pts for the exercises in Part 1; 5pts for Part 2)
  * You will get partial credit for questions even if the answer is not correct, so it is better to attempt all questions first, and then return to any particular question that you are unsure about.


## Cheatsheets

You are encouraged to review and keep the following cheatsheets handy while working on this project:

*   [What graph should I make?][what-graph]

*   [dplyr cheatsheet][dplyr-cheatsheet]

*   [ggplot2 cheatsheet][ggplot2-cheatsheet]

*   [RStudio cheatsheet][rstudio-cheatsheet]

*   [R Markdown cheatsheet][rmarkdown-cheatsheet]

*   [R Markdown reference][rmarkdown-reference]

[what-graph]:           https://drive.google.com/file/d/1zsedQ9kHFtxhFE3R99PQ1a_yTTamJu3e/view?usp=sharing
[dplyr-cheatsheet]:     https://gmuedu-my.sharepoint.com/:b:/g/personal/dwhite34_gmu_edu/ESQlogUDLfpNiXc3cD40crwBz0C0zfESw-6jRwTrHT4UBA?e=bQKhzS
[ggplot2-cheatsheet]:   https://gmuedu-my.sharepoint.com/:b:/g/personal/dwhite34_gmu_edu/ESLxplzb1sdLszfqs3208G0BdScfSbNqrikzJ1pIKczsFw?e=cwYcjM
[rstudio-cheatsheet]:   https://gmuedu-my.sharepoint.com/:b:/g/personal/dwhite34_gmu_edu/EVAQYYLorhxPh49NdlZV4KgBNNBQHRdJNthHK0ZuID8_Gw?e=dfzJPt
[rmarkdown-reference]:  https://gmuedu-my.sharepoint.com/:b:/g/personal/dwhite34_gmu_edu/Ed4VQ0-6mEhBp2IkjIdGDK0BwaR9BDzEnpnVyyxDn_gasg?e=1eLHsa
[rmarkdown-cheatsheet]: https://gmuedu-my.sharepoint.com/:b:/g/personal/dwhite34_gmu_edu/ETKKUWqePhRJv-VvAOsg4F4BPte7yKfQJKyyr1gNMg46yQ?e=hJPHXV
