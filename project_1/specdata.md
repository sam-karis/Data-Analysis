### Introduction

Hello aspiring data scientists and analysts. **WELCOME** to a career
path full of suprises and discoveries that are changing the world
everyday. Even with the high value that the discipline of data science
comes with, there is a big shortage of data scientist expert in the
world and we need everyone we can get.

In this small project I introduce you into data analysis with R
programming. It is a step by step tutorial of viewing, importing and
exploring your data from one and multiple csv files to enable you answer
questions and even make recommendations.

Originally our data set is contained in zip file named
**rprog%2Fdata%2Fspecdata**. The zip file contains 332
comma-separated-value (CSV) files containing pollution monitoring data
for fine particulate matter (PM) air pollution at 332 locations in the
United States. Each file contains data from a single monitor and the ID
number for each monitor is contained in the file name. For example, data
for monitor 200 is contained in the file "200.csv".

**Specdata** - Is the name of the folder we store our csv files after
extracting our zip file, thus we will use it us our directory through
out this tutorial.

Each csv file contains four variables:

-   Date: the date of the observation in YYYY-MM-DD format
    (year-month-day)

-   Sulfate: the level of sulfate PM in the air on that date (measured
    in micrograms per cubic meter)

-   Nitrate: the level of nitrate PM in the air on that date (measured
    in micrograms per cubic meter)

-   Id

Enough with the stories lets dive into our exploration and figure out
what our data is all about through R.

Befor any beginning analysis you must always remember to check the
directory you are working on for easy access of your work in future.

To do this we call **getwd()** function in R that will return our
current directory.

    getwd()

Incase you not comfortable to save your work in your current directory
you can use the **setwd()** function to set the directory of your
choice.

### Getting Started

Assuming you are on your desired directory.  
Taking look at our original dataset file(**rprog\_data\_specdata.zip**)
it is in a compressed. This is a little problem since R cannot read a
zipped file directly, so you have to figure out how to unzip the file to
access and read the data into R.  
Lucky for you R has a function **unzip()** just for that purpose:

-   For this if you are working on console you can run the **unzip**
    function directly

-   Alternatively if you work on an R script like me, you can define a
    function that unzip the file and when you run the script as the
    source just call the function you defined.(*i.e. un\_zip() for my
    case*)

<!-- -->

    un_zip <- function(){
      unzip("rprog%2Fdata%2Fspecdata.zip")
    }
    un_zip()

For both cases the you create a new folder named(**specdata**) that
contain all the 322 csv files.

To confirm this lets create a character vector consisting of all the
file's names in the folder.

    all_file <- list.files("specdata", full.names = TRUE)

To know the total number of files find length of the vector .

    length(all_file)

    ## [1] 332

For a better understanding lets see a sample of the first and last five
file names using the **head()** and **tail** function.

    head(all_file, 5)#first 5

    ## [1] "specdata/001.csv" "specdata/002.csv" "specdata/003.csv"
    ## [4] "specdata/004.csv" "specdata/005.csv"

    tail(all_file, 5)#last five

    ## [1] "specdata/328.csv" "specdata/329.csv" "specdata/330.csv"
    ## [4] "specdata/331.csv" "specdata/332.csv"

### A step Further

This far we have only check our data files from outside lets go
deeper.  
Having a vector with names we can access each name by indexing the
vector, eg the name of the first file can be accessed as below.

    all_file[1]

    ## [1] "specdata/001.csv"

Nice this get us step closer to getting the data into R, with the file
name we can use the **read.csv()** function to read and import data into
R.

    file_1 <- read.csv(all_file[1])

Now we have all the data from "001.csv" as a R object named file\_1.

Any data you read into R using **read.csv** or **read.table** by dafault
craete a dataframe, just in case you can use the code below to confirm
the class of your object:

    class(file_1)

    ## [1] "data.frame"

That is good we can know easily get more information about our data
stored the dataframe(file\_1).

-   we start by getting the object dimension (number of rows and
    column):

<!-- -->

    dim(file_1)

    ## [1] 1461    4

The file contain 1461 rows and on 4 columns, I think it would be more
helpful if we got names of the 4 columns.

    colnames(file_1)

    ## [1] "Date"    "sulfate" "nitrate" "ID"

This not enough lets get more info about the dataframe, fortunately the
**str()** and **summary** functions work the magic.  
Lets give it a try.

    str(file_1)  

    ## 'data.frame':    1461 obs. of  4 variables:
    ##  $ Date   : Factor w/ 1461 levels "2003-01-01","2003-01-02",..: 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ sulfate: num  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ nitrate: num  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ ID     : int  1 1 1 1 1 1 1 1 1 1 ...

    summary(file_1)

    ##          Date         sulfate          nitrate             ID   
    ##  2003-01-01:   1   Min.   : 0.613   Min.   :0.1180   Min.   :1  
    ##  2003-01-02:   1   1st Qu.: 2.210   1st Qu.:0.2835   1st Qu.:1  
    ##  2003-01-03:   1   Median : 2.870   Median :0.4530   Median :1  
    ##  2003-01-04:   1   Mean   : 3.881   Mean   :0.5499   Mean   :1  
    ##  2003-01-05:   1   3rd Qu.: 4.730   3rd Qu.:0.6635   3rd Qu.:1  
    ##  2003-01-06:   1   Max.   :19.100   Max.   :1.8300   Max.   :1  
    ##  (Other)   :1455   NA's   :1344     NA's   :1339

Nice that so much information with just one line of command.  
As an analyst most times you need this summary to have a general
understanding about your data to be able to go on with more advance
analysis.

Lets give it a try by trying to answer some qustions.

1.  **Which polutant has a higher mean ?**

<!-- -->

    mean(file_1$sulfate, na.rm = TRUE)#sulfate mean

    ## [1] 3.880701

    mean(file_1$nitrate, na.rm = TRUE)#nitrate mean

    ## [1] 0.5499098

In the mean function setting **na.rm** to **TRUE** take cares of the
missing data (NA's), by default it is always **FALSE** and hence if one
fails to explictly set na.rm to TRUE we will always get NA as the result
if the data referenced contain one or more missing data. This can be
seen below:

    mean(file_1$sulfate)# sulfate mean without removing the NA'S

    ## [1] NA

From the mean we can tell the sulfate mean is quite large compared to
that nitrate mean, that answers our quetion.  
Data scientist our first intution is to question data and try to get the
answers so lets try another:

1.  **Is nitrate related to sulfate ?**

For this lets find the correlation between the nitrate and sulfate as
shown below.

    cor(file_1$sulfate, file_1$nitrate, use = "pairwise.complete.obs")

    ## [1] -0.2225526

Wonderful in we have the correlation thus we can answer our question.

A negative correlation means there exists an inverse relationship
between nitrate and sulfate.  
**-0.2226** is close to zero than to one thus we can conclude that the
exist a small inverse relationship between sulfate and nitrate.

### Lets Bring it all together

So far we have dealt with first monitor data only contained in "001.csv"
file, which is one out of the 332 monitors involved.  
Now lets figure a way to combine two or more csv files data for a more
comprehensive and representative analysis. If you try to analyse each
file as we did you to end up with numerous inconclusive results.  
Fortunately R has solutions for such situation because they are quit
common in data world.  
For this case I choose to utilize the functions and loops features in R.

-   **Functions** : This is a very powerful feature in R that eneble us
    to write and reuse our code every time we need to repeat a similar
    task.  
-   **Loops** : Another powerful feature that enable us to go through
    verious elements in R objects performing same task to each. e.g.  
    &gt; Looping through a vector with the names of the csv files to
    read it's data into R

Data scientist final goal is to answer questions and give
recommendations.  
Lets dive into loops and function concepts by first difining some
questions that we will answer. Having well defined question keep you
focused during your analysis as they remind you what you have to achieve
at the end of your analysis.

1.  How many complete (not missing) and missing obseravations data
    collected by monitors ?

For a single monitor data it is very easy, we only need to utilize two
basic functions in R i.e. **sum $ complete.cases** as below:

    sum(complete.cases(file_1))# complete observations

    ## [1] 117

    sum(!complete.cases(file_1))# missing observation

    ## [1] 1344

This may not help alot if we seek to understand into more details. For
instance we may want to know and compare the number of complete
observations in data collected by the 1, 50, 70, 200 or any sample of
monitors you may be interested in.  
To achieve this we need to find away to loop though the various
monitor's data to compute the number of complete and missing
observations to return a more informative result.

To do this lets define a function that does that work for us.

    complete <- function(directory, id = 1:332){
      all_file <- list.files(directory, full.names = TRUE)
      res = data.frame()
      for(i in id){
        dat1 <- read.csv(all_file[i])
        y <- sum(complete.cases(dat1))
        x <- sum(!complete.cases(dat1))
        res <- rbind(res, data.frame(Id = i, Complete_obs = y, missing_obs = x, Total_obs= y+x) )
      }
      res
    }

Lets try to understand our function above:

-   It takes two arguements tha directory contain our datasets and the
    indexes of the monitors we are interested in.  
-   Creates an a character vector containing names of the csv files and
    initialize an empty dataframe where we will store our result.  
-   Use a for loop to go through each csv file to compute the complete,
    missing observations from each monitor and store the result into
    dataframe initialized.  
-   Return the dataframe that has the results from all monitors.

Lets use or function to some result.  
first lets get the result for the first 5 monitors.

    complete("specdata", 1:5)

    ##   Id Complete_obs missing_obs Total_obs
    ## 1  1          117        1344      1461
    ## 2  2         1041        2611      3652
    ## 3  3          243        1948      2191
    ## 4  4          474        3178      3652
    ## 5  5          402        2520      2922

Nice what about the 1st, 100th, 250th and 332nd monitor.

    complete("specdata", c(1,100,250,332))

    ##    Id Complete_obs missing_obs Total_obs
    ## 1   1          117        1344      1461
    ## 2 100          104         992      1096
    ## 3 250          180        1281      1461
    ## 4 332           16         715       731

Great the function works perfectly you can enter different indexes to
try and find if there is any unique pattern between different monitors.

Lets move to defining our next question.

1.  what is the mean of each pollutant(sulfate and nitrate) from all or
    some of the monitors data?

To achieve this we need to find away to loop though the various
monitor's data read, and combine the data to create one huge dataframe
containing all the data.

For this problem we will approach it by defining a function that takes
three arguement that is:

-   The the name of the directory that contain the datasets  
-   The polluntant we want to get the mean  
-   The Id for the monitors we wish to involve (optional)

The function then returns a mean.  
Below is our function:

    pollutantmean <- function(directory, pollutant, id= 1:332){
      all_file <- list.files(directory, full.names = TRUE)
      dat = data.frame()
      for(i in id){
        dat <- rbind(dat,read.csv(all_file[i]))
      }
      mean(dat[[pollutant]], na.rm = TRUE)
    }

Generally the function read the datasets, create a character vector with
names of the csv files names, create a new dataframe that combine all
the data from csv indexed by ID argurment.

The function is not optimized thus might be slow and will be improving
it in my upcoming tutorial.

Lets understand our function by running some sample.  
First lets get the mean of sulfate and nitrate from all monitors.

    pollutantmean("specdata", "sulfate") #overall sulfate mean

    ## [1] 3.189369

    pollutantmean("specdata", "nitrate") #overall nitrate mean

    ## [1] 1.702932

So from the result sulfate mean is higher than the nitrate mean.

To explore more we may wish to get the mean of the first ten monitors.

    pollutantmean("specdata", "sulfate", 1:10)# sulfate mean 

    ## [1] 4.064128

    pollutantmean("specdata", "nitrate", 1:10)# nitrete mean

    ## [1] 0.7976266

Nice you have a fuction you can choose different monitors and explore
more to see if the mean significantly varies for certain monitors.

1.  What is the relationship between sulfate and nitrate ?

Relationship can be explained by several statistical measures for this
case I choose to use correlations.  
To dive in lets define a function that create one huge dataframe
consisting the data for all monitors.

    correlation <- function(directory, threshold = 0){
      all_file <- list.files(directory, full.names = TRUE)
      dat = data.frame()
      for (i in 1:322){
        dat1 <- read.csv(all_file[i])
        if(sum(complete.cases(dat1)) > threshold){
          dat <- rbind(dat,dat1)
        }
      }
      if (nrow(dat) == 0){
        message("no mentor with that threshold")
      }else{
      cor(dat[[2]], dat[[3]], use = "pairwise.complete.obs")
      }
    }

By default if you only pass one argument that is the directory you get
your correlation of all monitors as below.

    correlation("specdata")

    ## [1] 0.05875351

From our answer we can tell that there is a sightly small positive
relationship between sulfate and nitrate.  
This is great as it answer our question but as a data scientist you
should always be ready and eager to go a step further to discover a
hidden pattern or characteristic about data.

For this in our correlation function I provided a second optional
arguement to enable us explore more from the data with the a same
function.  
For instance you might want to know the correlation of sulfate and
nitrate, but only include data of monitors who had more than 200
complete observations. To achieve you just pass the second argument as
200 i.e.

    correlation("specdata",200)

    ## [1] 0.05804314

what about a larger threshold like 1000.

    correlation("specdata",1000)

    ## [1] 0.05438838

Nice one all this correlation seem to range around 5% - 6%. But what if
we try a even a larger threshold like 3000 complete observations.

    correlation("specdata",3000)

    ## no mentor with that threshold

This one throws back a message instead of correlation, if you go back to
our function this is the message we defined to handle any situation a
threshold is not reached by any monitor.

> Thanks for checking my R tutorial hope it was of helpful look for more
> advance concept and efficient way to optimize your analysis comming
> soon that will external packages.
