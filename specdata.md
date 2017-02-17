### Introduction

Hello aspiring data scientists and analysts, for every journey you have
to begin somewhere lets me be part of you start we all know there is a
shortage of data scientist in the world and we need everyone we can get.

**WELCOME** in this small project I introduce you into R programming,
step by step from reading, exploring, manipulating your data from
multiple csv files to enable you answer questions and even make
recommendations.

**Specdata** - Is the name of the folder containing our dataset thus we
will use it us our directory through out this tutorial.

The zip file contains 332 comma-separated-value (CSV) files containing
pollution monitoring data for fine particulate matter (PM) air pollution
at 332 locations in the United States. Each file contains data from a
single monitor and the ID number for each monitor is contained in the
file name. For example, data for monitor 200 is contained in the file
"200.csv".

Each file contains four variables:

-   Date: the date of the observation in YYYY-MM-DD
    format (year-month-day)

-   Sulfate: the level of sulfate PM in the air on that date (measured
    in micrograms per cubic meter)

-   Nitrate: the level of nitrate PM in the air on that date (measured
    in micrograms per cubic meter)

-   Id

Enough with the stories lets dive into the data and try to figure all
that through R.

First lets check the directory we are on for us to be sure if it is the
location we want to save our work.  
To do this we use the function **getwd()**

    getwd()#get working directory

    ## [1] "C:/Users/Muriuki/Desktop/sam/R/R_program"

*Above is my current directory so do not be worried when you run the
function and get a different diferent directory *

*Just incase you not comfortable to save your work in your current
directory you can use the function **setwd()** to set the directory of
your choice *

### Getting Started

Once you are on your desired directory.  
Taking a look at our data set(**rprog\_data\_specdata.zip**) it is in a
compressed format.  
This is a little problem because R cannot read a zipped format, so we
have to figure out how to unzip the data set for us be able to access
and read the data into R.  
Lucky for us R has a function **unzip()** just for that purpose:

-   For this if you are working on console you can run the **unzip**
    function directly

-   Alternatively if you work on an R script like me, you can define a
    function that unzip the data set and when you run the script as the
    source file just call the function you defined.(*i.e. un\_zip() for
    my case*)

<!-- -->

    un_zip <- function(){
      unzip("rprog_data_specdata.zip", exdir = "specdata")
    }
    #un_zip()

-   For both cases the you achieve to create a new folder(**specdata**
    for my case) that contain all the 322 cs v files.

-   To confirm this lets create a list of the files names in the folder

<!-- -->

    all_file <- list.files("specdata", full.names = TRUE)

We can find length of the list to know the total number of files they
are in total.

    length(all_file)

    ## [1] 332

Just a further lets see a sample of the first ten file names using the
**head()** function.

    head(all_file, 10)

    ##  [1] "specdata/001.csv" "specdata/002.csv" "specdata/003.csv"
    ##  [4] "specdata/004.csv" "specdata/005.csv" "specdata/006.csv"
    ##  [7] "specdata/007.csv" "specdata/008.csv" "specdata/009.csv"
    ## [10] "specdata/010.csv"

### A step Further

This far we have only check our data from outside lucky for us we have
all the names of the files names in a list thus can use them their
indexing to get the name.  
Example the name of file one can be accessed as below.

    all_file[1]

    ## [1] "specdata/001.csv"

With the name we are a step closer to getting the data into R using the
**read.csv()** function use below.

    file_1 <- read.csv(all_file[1])

Now we have all data "001.csv" into R in an object named file\_1.  
Any data you read into R using **read.csv** or **read.table** by dafault
craete a dataframe, just in case you can use the code below to confirm:

    class(file_1)

    ## [1] "data.frame"

That is good we can know easily get more information about the
dataframe.

-   we start by getting the number of rows and column:

<!-- -->

    dim(file_1)

    ## [1] 1461    4

Nice that file contain 1461 rows and on 4 columns, now i think it would
be great if we got names of the 4 columns.

    colnames(file_1)

    ## [1] "Date"    "sulfate" "nitrate" "ID"

I think it would be better if we got all the above info by just one
function fortunately the **str()** and **summary** can work the magic.  
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

Nice that so much information with just one line of command, but not all
times you need all those measures in your analysis.  
Lets see how to go about this for example you may want to get the mean
of sulfate or nitrate only, to answer a qustion.  
**Which polutant is higher than the other ?**

    mean(file_1$sulfate, na.rm = TRUE)#sulfate mean

    ## [1] 3.880701

    mean(file_1$nitrate, na.rm = TRUE)#nitrate mean

    ## [1] 0.5499098

In the above mean function setting **na.rm** to **TRUE** take cares of
the missing data (NA's), by default it is always **FALSE** and hence if
one fails to explictly set na.rm to TRUE we will always get an NA result
if the data referenced contain missing data. This can be seen below:

    mean(file_1$sulfate)# sulfate mean without removing the NA'S

    ## [1] NA

From the mean we can tell the sulfate mean is quite large compared to
that nitrate mean, that answers our quetion.  
As a data scientist our first intution is to ask questions and try to
get the answers for example:  
**Is nitrate related to sulfate ?**

while we can answer above question by finding the correlation between
the nitrate and sulfate as shown below.

    cor(file_1$sulfate, file_1$nitrate, use = "pairwise.complete.obs")

    ## [1] -0.2225526

Wonderful in we have the correlation thus we can answer our question.

A negative correlation mean the exist a inverse relationship between
nitrate and sulfate.  
**-0.2226** is close to zero than to one thus we can conclude that the
exist a small inverse relationship between sulfate and nitrate.

### Lets Bring it all together

So far we have been dealing with one monitor's data that is "001.csv"
file, out of the 332 monitors involved.  
Now lets figure a way to combine two or more csv files for a
comprehensive analysis. This may seem like a challenge do we have to
analyse each file as we did at a time to end up with numerous
inconclusive results.  
Worry not R has solutions. R simplify the work for it users you only
have to understand features R provides to you.  
For this case I choose to utilize the functions and loops features in R.

-   **Functions** : This is a very powerful feature in R that eneble us
    to write and reuse our code every time we need to repeat a similar
    task.  
-   **Loops** : Another feature that enable us to go through verious R
    objects, element performing the same task e.g.  
    &gt; Looping through a vector with the names of the csv files to
    import the data into R

For any data scientist their final goal is to answer a question and give
recommendations, so to dive into this concepts lets difine some
questions that will keep us focused to what we want to achieve at the
end of our analysis.

1.  How many complete (not missing) and missing obseravations data
    collected by monitors ?  
    While for a single dataset by one monitor it is very easy, we only
    need to utilize two basic functions in R i.e. **sum $
    complete.cases** as below:

<!-- -->

    sum(complete.cases(file_1))# complete observations

    ## [1] 117

    sum(!complete.cases(file_1))# missing observation

    ## [1] 1344

This does not fully solve our problem for instance when we need to know
the number of complete observations in data collected by the first 30,
50, 200 or any numbers of monitors you may be interested.  
To achieve this we need to find away to loop though the various
monitor's datasets, read, compute compute the complete and missing
observations then combine then store the result and return after the
operation is complete.  
To do this lets write a function that does that work for us.

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
monitor's datasets, read, and combine the data to create one huge
dataframe containing all the data.

For this problem we will approach it by defining a function that takes
three arguement that is:

-   The the name of the directory that contain the datasets  
-   The polluntant we want to get the mean  
-   The Id for the monitors we wish to involve

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

Generally the function read the datasets, create a list of names of the
csv files names, create a new dataframe that combine all the data from
csv indexed by ID argurment.

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
    In analysis relationship can be explaine several statistical
    measures for this case I choose to use correlations.  
    To dive in lets define a function that create one huge dataframe
    consisting the data for all monitors.

<!-- -->

    correlation <- function(directory, threshold = 0){
      all_file <- list.files(directory, full.names = TRUE)
      dat_b = data.frame()
      for (i in 1:322){
        dat1 <- read.csv(all_file[i])
        if(sum(complete.cases(dat1)) > threshold){
          dat_b <- rbind(dat_b,dat1)
        }
      }
      if (nrow(dat_b) == 0){
        message("no mentor with that threshold")
      }else{
      cor(dat_b[[2]], dat_b[[3]], use = "pairwise.complete.obs")
      }
    }

By default you only have to pass one argument that is the directory
where your un\_zipped datasets are stored and you get your correlation
as below.

    correlation("specdata")

    ## [1] 0.05875351

And from our answer we can tell that there is a sightly small positive
relationship between sulfate and nitrate.  
This is great discovery from but any data scientist has an extra
intution of going a step further to discover a pattern or characteristic
about data.  
For this in our correlation function I provided a second optional
arguement to enable us explore and get more information from the data
with the a same function.  
For instance you might want to know the correlation of sulfate and
nitrate but you want only to include monitors who had more than 200
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

While that one throws back a message instead of correlation, if you
remember in our this is the message we defined to communicate to the us
when we enter a threshhold that no monitor reached.

> Thanks for checking my R tutorial hope it was of help to begginers
> look for more advance concept comming soon that will external
> packages.
