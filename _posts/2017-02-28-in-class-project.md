---
layout: post
title: "February 28, 2017  In-Class"
tags: [R, RMarkdown, Functions, Github]
---



# Part 1: Getting Your Website Live

- This is sich a new feature of R that the documentation has not been fully created. 
- The method used to get this live is what I have found works for me but I am hoping a better way will come: 

1. Open up your `git` command window. 
2. Use the `cd` command to enter the `public` folder of your githubpage you made. on my computer this is:
```
cd Dropbox/PHP2560/GitHubPage/public
```
3. Start a `git` there:
```
git init
```
4. Add origin
``` 
git remote add origin https://github.com/username/username.github.io
```
5. Add everything you have created so far:
```
git add .
```
6. Save your first commit
```
git commit -m "My First page commit"
```
7. Push to git:
```
git push origin master
```
8. Check out your website: https://username.github.io

- Every time you make changes you can update by adding everything in the public folder and committing. 





# Part 2: Building Functions



[Click to view Markdown](https://github.com/php2560/inclass/blob/master/2017-02-28-february-28-2017-in-class-project.Rmd)

**Background.**
Now-common ideas like "early adopters" and "viral marketing" grew from sociological
studies of the diffusion of innovations.One of the most famous of these studies
tracked how a then-new antibiotic, tetracycline, spread among doctors in four small
cities in Illionis in the 1950s. In this lab, we will go back to that data to look
at one of the crucial ideas, that of the innovation (prescribing tetracycline)
"spreading" from person to person.

We will work with 2 datasets:

* [ckm_nodes.csv](https://drive.google.com/uc?export=download&id=0B8CsRLdwqzbzN2V3ZHQtSzMySXM)
* [ckm_nodes.csv](https://drive.google.com/uc?export=download&id=0B8CsRLdwqzbzbmE2QU5keGgyQzQ)

The former has information about each individual doctor in the four towns. The latter records  which doctors knew each other.

##Part 1

1. Load the data in `ckm_nodes.csv` into a data frame, called `ckm_nodes`. Check that it has 246 rows and 13 columns. Check that there are columns  named `city` and `adoption_date`.

2. `adoption_date` records the month in which the doctor began prescribing tetracycline, counting from November 1953. If the doctor did not begin prescribing it by month 17, i.e., February 1955, when the study ended, this is recorded as `Inf`.  If it's not known when or if a doctor adopted tetracycline, their value is `NA`. How many doctors began prescribing tetracycline in each month of the study?  How many never prescribed?  How many are `NA`s? 

3. In the object `ckm_nodes`, the `adoption_date` column records the month in which the doctor began prescribing tetracycline, counting from November 1953. If the doctor did not begin prescribing it by month 17, i.e., February 1955, when the study ended, this is recorded as `Inf`. If it's not known when or if a doctor adopted tetracycline, their value is `NA`.  Create a vector which records the index numbers of doctors for whom `adoption_date` is not NA.  Check that this vector has length 125.  Create a data frame, `cleaned_nodes`, which contains only those rows of `ckm_nodes`.  (Do not drop rows if they have a value for `adoption_date` but are NA in some other column.)  **Use `cleaned_nodes`, rather than `ckm_nodes`, for the rest of the lab.**

4. Write a function, `adopters`, which takes two arguments, `month`, with no default value, and `not.yet`, defaulting to `FALSE`.  If `not.yet` is `FALSE`, `adopters` should return a Boolean vector, indicating the doctors who began prescribing tetracycline in that month.  If `not.yet` is `TRUE`, then `adopters` should return the vector indicating the doctors who began prescribing _after_ that month (or never did).  Check that `adopters(2)` indicates 9 doctors began prescribing in month 2, and that `adopters(month=14,not.yet=TRUE)` indicates that 23 doctors began prescribing after month 14, or never did.

5. The object `ckm_network` is a binary matrix; the entry in row $i$, column $j$ is 1 if doctor number $i$ said that doctor $j$ was a friend or close professional contact, and 0 otherwise. Create a new matrix, `clean_network`, which drops the rows and columns corresponding to doctors with missing `adoption_date` values. Check that the result has 125 rows and columns.  **Use this reduced matrix, and its row and column numbers, for the rest of the lab.**

6. Create a vector which stores the number of contacts each doctor has.  Use an `apply` function rather than a loop.  Check that doctor number 41 had 3 contacts.



##Part 2

7. _Counting Peer Pressure_
    a. Write a function, `count_peer_pressure`, which takes in the index number of a doctor and a month,  and returns the number of doctors whom that doctor named as contacts, _and_ had begun prescribing tetracycline by that month or earlier.  If it is working properly, doctor number 37 and month 5 should return 3.
    
    b. Write a function, `prop_peer_pressure`, which  takes in the index number of a doctor and a month, and returns the proportion of the doctor's contacts who are already prescribing tetracycline by that month.  If a doctor has no contacts, your function should return `NaN`.   Check that doctor 37, month 5 returns a proportion of 0.6, but doctor 102 in month 14 returns `NaN`. Your function should call, not repeat, your function from (7a), and use your vector from (6).
    
8. _Averaging Peer Pressure_
    a. Write a function which takes in a month, and returns a vector of length two.  For the first element, consider doctors who _began_ prescribing in that month, then return the average proportion of prescribers among their contacts.  For the second element, , consider doctors who _began_ prescribing after that month (or never), then return the average proportion of prescribers among their contacts.  Call your code from (4) and (7); use an `apply` function rather than a loop if at all possible.   _Hint_: `mean` has an `na.rm` option.
    
    b. Compute the average proportions from (8a) for each month in the study. Use an `apply` function rather than a loop if you can. Plot the two average proportions from (8a) over time, and in a second plot show their difference.  Do the doctors who adopt in a given month consistently have more contacts who are already prescribing than the non-adopters?


#####Data Notes


The original study was published as

> James Coleman, Elihu Katz and Herbert Menzel, "The Diffusion of an 
Innovation Among Physicians" _Sociometry_ **20** (1957): 253--270.

The files used here are taken from http://moreno.ss.uci.edu/data.html#ckm with some formatting changes.  CKM actually measured three types of link among the doctors --- friendship, general discussion, and asking for medical advice.  To keep things simple, we are combining all three types of tie, and treating them as symmetric.
