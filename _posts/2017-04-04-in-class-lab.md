---
layout: post
title: "April 4, 2017  In-Class"
tags: [R, RMarkdown, SQL, Github]
---



```{r, echo=FALSE}
knitr::opts_chunk$set(
  tidy=TRUE,     # display code as typed
  size="small",
  eval=F)
```


## Amazon Services

For the first part of this lab we will work on setting up your own Database on amazon. 


### Getting started:

- Go to the [DataCamp Course](https://www.datacamp.com/courses/1118/)
    - Watch the [Setting up Amazon Webservices Video in Chapter 12](https://campus.datacamp.com/courses/php-2560-statistical-computing/5607?ex=4)
    - Watch the [Creating a Database in AWS](https://campus.datacamp.com/courses/php-2560-statistical-computing/5607?ex=5)
    - Watch the [Connecting into your AWS database](https://campus.datacamp.com/courses/php-2560-statistical-computing/5607?ex=5)
- Follow the steps below to work with this database.


The goal will be to use this lab to work on the titanic dataset. The code below is similar to how you will connect. 

```{R}
library(RMySQL)
con <- dbConnect(MySQL(),
                 user = 'php2560',
                 password = 'password',
                 host = 'php2560.ca5lkwatfvxo.us-east-1.rds.amazonaws.com',
                 dbname='titanic')
```
.

```{R}
query <- "show tables;"
dbGetQuery(con, query)
```

1. Go to Kaggle and downlaod the titanic datasets:
    - [https://www.kaggle.com/c/titanic/data](https://www.kaggle.com/c/titanic/data)
    


2. Combine the csv files and then read them into R

```{r}
titanic <- read.table("path/to/file/titanic.csv", header=TRUE, sep=",")
```

3. Create a table in MySQL from this data. Name it `titanic_yourlastname`:

```{r}
dbWriteTable(con, "titanic_yourlastname", titanic)
```

4. Now what tables are in this database?



## Basic usage of commands

We will start to use a list of commands that would be commonly used in MySQL:

These are:

- show tables;
- describe titanic_yourlastname;
- select name, pclass, age, sex from titanic limit 10;
 


We can run any of these commands as follow:

```
query <- "show tables;"
dbGetQuery(con, query)
```

Try these commands and test this out. 


## R and MySQL

5. Run the following SQL command. Before doing so describe what this is asking
```
select pclass, survived, avg(age) as avg_age from titanic_yourlastname   
   group by pclass, survived;
```


6.  Run the following SQL command. Before doing so describe what this is asking
```
select pclass, survived, avg(age) as avg_age from titanic_yourlastname
   where survived=1
   group by pclass, survived;
```


7. Run the following SQL command. Before doing so describe what this is asking
```
select name, pclass, survived, age from titanic_yourlastname 
   where name regexp '^.sen.+';
   ```

8. Run the following SQL command. Before doing so describe what this is asking
```
select name, pclass, survived, age from titanic_yourlastname 
   where name regexp 'Jakob.*$';
```

9.  Run the following SQL commands. What is different about them? What do they return?
```
select name, pclass, survived, age from titanic_yourlastname 
   where name regexp 'Ivar.*$';
   ```
   
```
select name, pclass, survived, age from titanic_yourlastname 
   where name regexp ',.*Ivar.*$';
```


10. We can also plot data from this:
```{r}
myQuery <- "select pclass, avg(fare) as avg_fare from titanic_yourlastname
              group by pclass;"
myData <- dbGetQuery(con, myQuery)
library(ggplot2)
ggplot(myData, aes(pclass, avg_fare)) + geom_bar(stat="identity")
```



## More challenging

11. Create a data frame with:
- `pclass`
- `sex`
- `age`
- `fare`
- `survived`
- `parch`
Only for people who did not survive. 

12. Create a graph of the average survival over the different classes. Make two distinct lines for male and female passengers. Create this plot using ggplot. 






## Using MonetDbLite

Click on the link below to access the github repository for the rest of the lab:

- [In Class github](https://classroom.github.com/assignment-invitations/d5d68f44117ab30bcf0dbda90b040e1b)