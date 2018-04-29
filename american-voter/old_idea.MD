Proposal for a final project in DS-GA3001-015. Jan Zilinsky.

*Document in progress. Status: I have collected most of the [election data](https://github.com/zilinskyjan/US_economy_and_voting/blob/master/voting/election_results/state_vote_shares_by_party.csv), and I am experimenting with a methodological innovation (displaying output from a series of regression trees in a readable manner.)*

---

*Theme:* **U.S. voting behavior over time (1916-2016). How competitive are U.S. presidential elections?**

The goal of the project is two-fold:

1. I want to make a methodological contribution. There is a common assumption that regression trees can be visualized only when a single tree is displayed. As such, a well-known Machine Learning textbook [suggests](http://www-bcf.usc.edu/~gareth/ISL/ISLR%20Sixth%20Printing.pdf) that random forests, or bootstrap-aggregating methods are not conducive to visualization. My goal is to show that a group of the trees can be visualized (without unnecessary clutter). More details below.

2. The second goal is to explore voting patterns over time. I have collected Democratic and Republican vote share data for most U.S. states between 1916 and 2016. Among the question I plan to ask are:

- If we interactively modify what thresholds count as "close results", how has the proportion of "swing states" changed over time? This should be shown with a Shiny App.
- How well does the state of the economy predict election outcomes? 
  - Where will regression trees identify the relevant unemployment rate thresholds?
  - Measure the extent to which an incumbent was "punished" by voters for a bad economy not only by counting the number of states that flipped but, more importantly, by a change in the incumbent party vote share relative to the previous election.

Inputs: 

- [Election results](https://github.com/zilinskyjan/US_economy_and_voting/blob/master/voting/election_results/state_vote_shares_by_party.csv)
- Historical [unemployment rate](https://www.bls.gov/lau/staadata.txt) data from the BLS.
- Various [economic data that I collected](https://github.com/zilinskyjan/US_economy_and_voting).
- Use Gallup data from 2008 to 2017 to show state-level patterns of:
  - Subjective economic evaluations of citizens
  - Economic expectations

To summarize:

- I hope to show that visualization is useful not only to show data, but also to **display results from a machine learning procedure**.
- I will produce a self-contained Shiny app that will show how the number "swing states" hinges on what we choose to count as a competitive election (at the state level). I will do this for a longer time period than has been done before.
- I will include a handful of geographic visualizations or plots of election results which will hopefully tell an interesting story.

## Planning ahead

### Task 1: Build a Shiny app

Use historical data to display:

- How many states were competitive?
- Which states were competitive?
- Which states were *not* competitive for multiple elections in a row?

### Task 2: Visualize electoral outcomes of the last 20 presidential elections in the U.S.

Idea: prepare a heatmap to show all states over time, using Highcharter.

- Main outcome variables: Electoral results at the state and county-level
  - Democratic vote share
  - Republican vote share
  - Winning party
- Source: Leip Elections Atlas
- Economic data for the last 8 elections (1992-2016)

Idea 2: Dot plots (see below)

### To consider: prepare maps (TBD)

- Start with a leaflet map.
- Explore whether effective products can be created with
  - [statebins](https://rud.is/b/2017/11/18/statebins-reimagined/)
  - [tilegramsR](https://bhaskarvk.github.io/tilegramsR/); see also this [post](https://medium.com/google-news-lab/tilegrams-make-your-own-cartogram-hexmaps-with-our-new-tool-df46894eeec1)

Can use Census data to show: 

- Changes of employment in manufacturing.
- Changes of employment levels in construction.

## Preliminary work

Election results can be effectively visualized with dot plots. Here is an early version showing each county outcome in the 2016 election, which displays the extent of the drift toward the Republican nominee in 2016, relative to Obama's result in 2012. The exception is evident at the bottom, namely Utah:




## Methodological innovation: visualizing a large number of regression trees

Suppose that Y = vote share for the Republican in an election when a Democrat is the incumbent. The vote share could be at the state or county level.

Let X = labor market outcomes in the state of county.

When the labor market outcome, e.g. the unemployment rate, are worse, retrospective voting theories suggest that the incumbent's party will be punished. But just *how much* will the unemployment rate have to deteriorate for it to matter for election results? Regression trees are a useful agnostic procedure for identifying such thresholds.

Imagine that a simple (pruned) regression tree finds that a Democrat did worse relative to 4 years before when the county-level unemployment rate was 6% or higher. Normally, bagging would be used to improve predictions of Y; but suppose that instead of predicting election results at the state or county level we want to know *how robust/fragile the splits X are*. From each boostrapped sample of observations, the tree will choose an optimal threshold. So after 100 trees, cutoff points might be {5.95,5.99,6,7,...}. Clearly, if instead of saving \hat{Y} after each tree we carefully save the optimal splits, we will have a distribution of thresholds. And these can be visualized. It will be quite informative to see whether the range of these values is narrow (i.e. it is generally the case that people become more critical of the incumbent when the unemployment rate is 6%) or whether there is wide variation (in that case, different Americans in different locations have distinct expectations and their personal benchmarks for what counts as a "bad labor market" would differ).

This assumes, of course, that the labor market outcome just discussed is the top split. The initial variable chosen to grow a tree is crucial (and partly motivates bagging). If different variables are chosen initially, the problem becomes less tractable, and I will discuss the issues that arise. The method I propose may not work universally, but in spite of this complication I believe it could still be quite informative.

---

## Reference

Dean Lacy & Zachary D. Markovich: [Why Don’t States Switch Sides Anymore? The Rise and Fall of American Electoral Volatility](https://cpb-us-east-1-juc1ugur1qwqqqo4.stackpathdns.com/sites.dartmouth.edu/dist/9/280/files/2016/10/Volatility.Simple.v8.pdf)
