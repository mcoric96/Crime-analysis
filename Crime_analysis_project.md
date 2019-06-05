### Introduction

This project is made for purpose of Data mining course - Mathematics and
Computer science department <br> Original data from Kaggle:
<https://www.kaggle.com/adamschroeder/crimes-new-york-city/version/1>
<br> Data : New York crime data <br> Objective : extraction of knowledge
related to crimes from dataset <br> General purpose of this project is
not classic classification of regression problems, but finding out
important features of crime nature in New York.

### Software

R programming language (version 3.6.0) <br> RStudio <br> Jupyter
Notebook

### R Libraries

    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(ggplot2)

    ## Registered S3 methods overwritten by 'ggplot2':
    ##   method         from 
    ##   [.quosures     rlang
    ##   c.quosures     rlang
    ##   print.quosures rlang

    library(DT)
    library(arules)

    ## Loading required package: Matrix

    ## 
    ## Attaching package: 'arules'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     recode

    ## The following objects are masked from 'package:base':
    ## 
    ##     abbreviate, write

### Data

    crimeData = read.csv(file = "./CrimeData.csv",header = T,sep = ',')
    crimeData[sample(x = 1:1048575,size = 15),] # a brief look at the data

    ##         day month year  time       Borough dayPart Latitude Longitude
    ## 18028    16    12 2015 17.00      BROOKLYN   12-17 40.69538 -73.94921
    ## 201845    2     8 2015 21.00 STATEN_ISLAND   17-22 40.59809 -74.08147
    ## 453401   22     1 2015 11.00      BROOKLYN   06-12 40.68215 -73.92741
    ## 938811   25     1 2014 18.75         BRONX   17-22 40.81736 -73.90933
    ## 941518   23     1 2014 13.50      BROOKLYN   12-17 40.69514 -73.97513
    ## 161337   31     8 2015 19.00      BROOKLYN   17-22 40.64668 -73.95449
    ## 233478   12     7 2015 23.83 STATEN_ISLAND   22-06 40.62714 -74.15664
    ## 453468   20     1 2015 20.00     MANHATTAN   17-22 40.81712 -73.94801
    ## 937332   23     1 2014 17.00 STATEN_ISLAND   12-17 40.61756 -74.08142
    ## 830225   19     4 2014 21.67         BRONX   17-22 40.88630 -73.84791
    ## 864721   24     3 2014 21.83      BROOKLYN   17-22 40.61564 -73.94768
    ## 205954   31     7 2015 23.00         BRONX   22-06       NA        NA
    ## 1044319  31    10 2013 20.00     MANHATTAN   17-22 40.70788 -74.01328
    ## 177177   20     8 2015 18.33     MANHATTAN   17-22 40.73222 -73.99861
    ## 45536    25    11 2015  0.02        QUEENS   22-06 40.74725 -73.89151
    ##                       offenseDescription
    ## 18028                            ROBBERY
    ## 201845                     GRAND_LARCENY
    ## 453401                      HARRASSMENT_
    ## 938811                    FELONY_ASSAULT
    ## 941518           MISCELLANEOUS_PENAL_LAW
    ## 161337           MISCELLANEOUS_PENAL_LAW
    ## 233478                      HARRASSMENT_
    ## 453468                     PETIT_LARCENY
    ## 937332      ASSAULT_AND_RELATED_OFFENSES
    ## 830225    OFF_AGNST_PUB_ORD_SENSBLTY_AND
    ## 864721  CRIMINAL_MISCHIEF_AND_RELATED_OF
    ## 205954                        SEX_CRIMES
    ## 1044319                    GRAND_LARCENY
    ## 177177                     MISSING_VALUE
    ## 45536       ASSAULT_AND_RELATED_OFFENSES
    ##                                                  pdDescription
    ## 18028                       ROBBERY_PERSONAL_ELECTRONIC_DEVICE
    ## 201845             LARCENY_GRAND_BY_ACQUIRING_LOST_CREDIT_CARD
    ## 453401                                     HARASSMENT_SUBD___5
    ## 938811                                    ASSAULT_UNCLASSIFIED
    ## 941518                                      CRIMINAL_CONTEMPT_
    ## 161337                                      CRIMINAL_CONTEMPT_
    ## 233478                                     HARASSMENT_SUBD___5
    ## 453468                          LARCENY_PETIT_FROM_OPEN_AREAS_
    ## 937332                                                 ASSAULT
    ## 830225                                  AGGRAVATED_HARASSMENT_
    ## 864721                                  MISCHIEF_CRIMINAL_OF_M
    ## 205954                                           SEXUAL_ABUSE_
    ## 1044319 LARCENY_GRAND_FROM_BUILDING_(NON_RESIDENCE)_UNATTENDED
    ## 177177                               FALSE_REPORT_UNCLASSIFIED
    ## 45536                                                  ASSAULT
    ##         crimeCompleted offenseLevel occurenceLocation
    ## 18028        COMPLETED       FELONY     MISSING_VALUE
    ## 201845       COMPLETED       FELONY     MISSING_VALUE
    ## 453401       COMPLETED    VIOLATION          FRONT_OF
    ## 938811       COMPLETED       FELONY            INSIDE
    ## 941518       COMPLETED       FELONY            INSIDE
    ## 161337       COMPLETED       FELONY          FRONT_OF
    ## 233478       COMPLETED    VIOLATION            INSIDE
    ## 453468       COMPLETED  MISDEMEANOR     MISSING_VALUE
    ## 937332       COMPLETED  MISDEMEANOR            INSIDE
    ## 830225       COMPLETED  MISDEMEANOR     MISSING_VALUE
    ## 864721       COMPLETED       FELONY          FRONT_OF
    ## 205954       COMPLETED  MISDEMEANOR          FRONT_OF
    ## 1044319      COMPLETED       FELONY            INSIDE
    ## 177177       COMPLETED  MISDEMEANOR     MISSING_VALUE
    ## 45536        COMPLETED  MISDEMEANOR     MISSING_VALUE
    ##               premiseDescription
    ## 18028         TRANSIT_NYC_SUBWAY
    ## 201845                    STREET
    ## 453401       RESIDENCE_APT_HOUSE
    ## 938811       RESIDENCE_APT_HOUSE
    ## 941518  RESIDENCE_PUBLIC_HOUSING
    ## 161337       RESIDENCE_APT_HOUSE
    ## 233478           RESIDENCE_HOUSE
    ## 453468        TRANSIT_NYC_SUBWAY
    ## 937332       RESIDENCE_APT_HOUSE
    ## 830225                    STREET
    ## 864721           RESIDENCE_HOUSE
    ## 205954                    STREET
    ## 1044319      COMMERCIAL_BUILDING
    ## 177177           PARK_PLAYGROUND
    ## 45536                     STREET

There is a difference beetwen this data and original from Kaggle. <br>
Simple preprocessing is made and some variables(date event was
reported,police jurisdiction...) are ejected, some are changed(date
variable to month, day and year, hours and minutes to time...) and some
are added(dayPart) due to simplicity. Variables "hours" and "minutes"
are joined into 1 continuous variable time - for instance: 15h 30min is
now 15.5 (15 + 30/60). <br> Variable "time" is divided into categorical
variable "dayPart" with 4 classes(parts of the day). <br> All NA's are
replaced with "MISSING\_VALUE"

    summary(crimeData)

    ##       day            month             year           time      
    ##  Min.   : 1.00   Min.   : 1.000   Min.   :1015   Min.   : 0.00  
    ##  1st Qu.: 8.00   1st Qu.: 4.000   1st Qu.:2014   1st Qu.: 9.00  
    ##  Median :15.00   Median : 7.000   Median :2014   Median :14.67  
    ##  Mean   :15.52   Mean   : 6.947   Mean   :2014   Mean   :13.51  
    ##  3rd Qu.:23.00   3rd Qu.:10.000   3rd Qu.:2015   3rd Qu.:19.00  
    ##  Max.   :31.00   Max.   :12.000   Max.   :2015   Max.   :23.98  
    ##  NA's   :65      NA's   :65       NA's   :65                    
    ##           Borough        dayPart          Latitude       Longitude     
    ##  BRONX        :227477   06-12:220746   Min.   :40.50   Min.   :-74.26  
    ##  BROOKLYN     :315648   12-17:279515   1st Qu.:40.67   1st Qu.:-73.97  
    ##  MANHATTAN    :244749   17-22:291612   Median :40.73   Median :-73.93  
    ##  QUEENS       :211958   22-06:256702   Mean   :40.73   Mean   :-73.93  
    ##  STATEN_ISLAND: 48743                  3rd Qu.:40.81   3rd Qu.:-73.88  
    ##                                        Max.   :40.91   Max.   :-73.70  
    ##                                        NA's   :32417   NA's   :32417   
    ##                         offenseDescription
    ##  PETIT_LARCENY                   :180246  
    ##  HARRASSMENT_                    :133179  
    ##  ASSAULT_AND_RELATED_OFFENSES    :114430  
    ##  CRIMINAL_MISCHIEF_AND_RELATED_OF:102771  
    ##  GRAND_LARCENY                   : 96232  
    ##  DANGEROUS_DRUGS                 : 56868  
    ##  (Other)                         :364849  
    ##                         pdDescription          crimeCompleted   
    ##  ASSAULT                       : 93214   ATTEMPTED    :  18758  
    ##  HARASSMENT_SUBD___5           : 89064   COMPLETED    :1029816  
    ##  LARCENY_PETIT_FROM_STORE_SHOPL: 57638   MISSING_VALUE:      1  
    ##  LARCENY_PETIT_FROM_BUILDING_UN: 47733                          
    ##  AGGRAVATED_HARASSMENT_        : 44820                          
    ##  HARASSMENT_SUBD_CIVILIAN      : 44116                          
    ##  (Other)                       :671990                          
    ##       offenseLevel        occurenceLocation 
    ##  FELONY     :327807   FRONT_OF     :230557  
    ##  MISDEMEANOR:585468   INSIDE       :549718  
    ##  VIOLATION  :135300   MISSING_VALUE:217752  
    ##                       OPPOSITE_OF  : 27135  
    ##                       OUTSIDE      :   445  
    ##                       REAR_OF      : 22968  
    ##                                             
    ##                 premiseDescription
    ##  STREET                  :320261  
    ##  RESIDENCE_APT_HOUSE     :229490  
    ##  RESIDENCE_HOUSE         : 97446  
    ##  RESIDENCE_PUBLIC_HOUSING: 79577  
    ##  OTHER                   : 28726  
    ##  COMMERCIAL_BUILDING     : 27212  
    ##  (Other)                 :265863

### Data visualization

The most crimes generally occur in Brooklyn while least number of crimes
occur in Staten Island.

    options(repr.plot.width=7, repr.plot.height=5)
    ggplot(data = crimeData) + geom_bar(mapping = aes(x = crimeData$Borough,color = Borough))+
    xlab("New York boroughs") + ylab("Num. of crimes")

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-4-1.png)

Distribution of "offense level" variable <br> Misdemeanor offense
dominate over felony and violation.

    options(repr.plot.width=5, repr.plot.height=5)
    ggplot(data = crimeData) + geom_bar(mapping = aes(x = crimeData$offenseLevel)) + xlab("Offense level")

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-5-1.png)

Simple correlations in data
---------------------------

Connection beetwen New York borough and offense level for newer
data(2012-2015)

    options(repr.plot.width=7, repr.plot.height=3)
    subset(crimeData,year >= 2012) %>% count(Borough , offenseLevel) %>% 
    ggplot(mapping = aes(x = Borough , y = offenseLevel)) + 
    geom_tile(mapping = aes(fill = n))

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-6-1.png)

Distribution of each offense level through the day. <br> It is clear
that second part of the day (17-21) is the time when most crimes of each
level occur and the morning is the part of the day with less crime
appearances.

    options(repr.plot.width=12, repr.plot.height=5)
    ggplot(crimeData , mapping = aes(x = time,colour = offenseLevel))+ 
    geom_freqpoly(binwidth = 0.9 , lwd = 1) + xlab("Time of the day")+ylab('Distribution')

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-7-1.png)

This trend don't change over time. <br> The similar pattern occur if one
smaller subset(44% of data) of data is taken (only crimes from last year
- 2015)

    options(repr.plot.width=9, repr.plot.height=5)
    subset(crimeData,year == 2015) %>%
    ggplot(mapping = aes(x = time,colour = offenseLevel))+geom_freqpoly(binwidth = 0.85 , lwd = 1) + 
    xlab("Time of the day")+ylab('Distribution')

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-8-1.png)

2014 year

    options(repr.plot.width=9, repr.plot.height=5)
    subset(crimeData,year == 2014) %>%
    ggplot(mapping = aes(x = time,colour = offenseLevel))+geom_freqpoly(binwidth = 0.85 , lwd = 1) + 
    xlab("Time of the day")+ylab('Distribution')

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-9-1.png)

Relation beetwen New York boroughs, offense level and time. <br> Crime
offense levels mostly don't depend on borough but on time od the day.

    options(repr.plot.width=10, repr.plot.height=7)
    qplot(data = subset(crimeData,year >= 2012) ,color =Borough, x = time , bins = 25 , ylab = "Distribution through time") + facet_grid(Borough~offenseLevel)

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-10-1.png)

Offense level vs. crime location <br> Inside crimes are dominant
independently of crime level.

    qplot(data = subset(crimeData,year >= 2013) , x = occurenceLocation,xlab = "Location of crime")+
    facet_grid(.~offenseLevel) + coord_flip()

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-11-1.png)

### How to get some new information - simple example

One of the main tasks of data mining is extraction new data and
information from the old one. Here is a very simple example of getting
day of the week from a given date.

    crimeData[15] <- crimeData[,c(3,2,1)] %>% apply(MARGIN = 1,
    FUN = function(vec){paste(vec,collapse = '-')}) %>% as.Date() %>% weekdays()
    names(crimeData)[15] <- 'weekDay'

### Day of the week vs. time (year 2014 and 2015)

As it can be seen, the most dangerous time of the week is weekend
night(saturday and sunday 22-06) and middle of the week through the day,
while the less dangerous is the middle of the week at night and weekend
mornings. For this and similar analysis, the idea is to track new trends
that might happen and for that reason newer data should be taken for
analysis(in this case last 2 years).

    subset(crimeData,!is.na(weekDay) & year >= 2014) %>% count(weekDay , dayPart) %>% 
    ggplot(mapping = aes(x = weekDay , y = dayPart)) + geom_tile(mapping = aes(fill = n))+
    xlab("Day of the week") + ylab("Part of the day")

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-13-1.png)

### Some things happen more often than the others - interactive data tables

For this kind of analysis DT(data-table) library is used - simple review
of particular desired events. <br> From this data-tables it is easy to
observe what events are more frequent than the others.

    subset(crimeData,year >= 2014 & occurenceLocation != "MISSING_VALUE") %>% 
    group_by(offenseDescription,Borough,dayPart,premiseDescription) %>% summarize(count = n()) %>% 
    arrange(desc(count)) %>% head(20) %>% datatable(options = list(pageLength = 10,scrollX='400px'))

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-14-1.png)

    subset(crimeData,year >= 2014 & occurenceLocation != "MISSING_VALUE") %>% 
    group_by(occurenceLocation,Borough,dayPart,pdDescription) %>% summarize(count = n()) %>% 
    arrange(desc(count)) %>% head(20) %>% datatable(options = list(pageLength = 10,scrollX='400px'))

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-15-1.png)

### Felony

2015 felony related crimes - small pattern emerges. <br> The most
dangerous place for this specific category is Brooklyn - 12 h, at
beginning of the week. The same pattern come up for 2014 year.

    subset(crimeData,year == 2015 & offenseLevel == "FELONY") %>% group_by(Borough,time,weekDay) %>% summarise(count = n()) %>% 
    arrange(desc(count)) %>% head(30) %>% datatable(options = list(pageLength = 10,scrollX='400px'))

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-16-1.png)

Association rules
-----------------

Association rules are rule-based data mining method for discovering
certain relations between variables in data-sets. The main purpose of
association rules is to discover strong rules in data-sets using
measures of interestingness. <br> Let
*I* = {*i*<sub>1</sub>, ..., *i*<sub>*n*</sub>} be the set of variables
in the dataset. Observations of data-set (rows of data frame) are
usually called **transactions**. A rule is defined like implication
*A* ⟹ *B* where *A*, *B* ⊂ *I*.<br> *A* is usually called antecedent or
left-hand-side (LHS) and *Y* consequent or right-hand-side (RHS). In
some implementations rule is defined like *A* ⟹ *i*<sub>*j*</sub> where
*i*<sub>*j*</sub> ∈ *I*.

### Significant measures

Let *X*, *Y* be itemsets, *X* ⟹ *Y* an association rule and *T* a set of
transactions of a given data-set.

### Support

Support is an indication of how frequently the itemset appears in the
dataset. <br> It is proportion of transactions(rows in data frame) that
contain specific itemset, with respect to number of transactions. <br>
$supp(X) = \\frac{|t \\in T ; X \\subset t|}{|T|}$ <br>

### Confidence

Confidence is an indication of how often the rule has been found to be
true.<br> $conf(X \\cup Y) = \\frac{supp(X \\cup Y)}{supp(X)}$ <br>
Confidence can be interpreted as an estimate of the conditional
probability *P*(*Y*|*X*), the probability of finding the *Y* in
transactions under the condition that these transactions also contain
the *X* in the left side of the rule.

### The lift

The lift of a rule is defined as
$lift(X \\implies Y) = \\frac{supp(X \\cup Y)}{supp(X) \* supp(Y)}$<br>
It is the ratio of the observed support to that expected if X and Y were
independent events. <br> If *X* and *Y* are truly independent events, we
can expect that about *s**u**p**p*(*X*)\**s**u**p**p*(*Y*) number of
transactions will contain both of them. <br> If the rule had a **lift of
1**, it would imply that the probability of occurrence of the antecedent
and that of the consequent are independent of each other. When two
events are independent of each other, no rule can be drawn involving
those two events. <br> If the **lift is &gt; 1**, that lets us know the
degree to which those two occurrences are dependent on one another, and
makes those rules potentially useful for predicting the consequent in
future data sets.<br> If the **lift is &lt; 1**, that lets us know the
items are substitute to each other. This means that presence of one item
has negative effect on presence of other item and vice versa. <br><br>
Definitons taken from :
<https://en.wikipedia.org/wiki/Association_rule_learning> <br> R
implementation: library arules , apriori algorithm.<br><br>

### Example - wrong way of using association rules

One of the obvious wrong ways of using of association rules is to apply
it to variables that are obviously correlated in some way. In this
dataset for instance we could get rule like: <br> offenseDescription =
ASSAULT\_AND\_RELATED\_OFFENSES ⟹ pdDescription = ASSAULT. It is clear
and natural that these 2 variables are in close relationship, so
although this rule might have large lift, is not very helpfull. For this
reason in examples below algorithm will take only some subset of
variables, excluding others that are obviously correlated with
them.<br><br>

### Apriori algorithm

Apriori algorithm is classic algorithm for generating association rules
from datasets or databases.<br> The key idea of the algorithm is to
begin by generating frequent itemsets with just one item (1-itemsets)
and to recursively generate frequent itemsets with 2 items, then
frequent 3-itemsets and so on till some stopping condition is satisfied.
<br> This is where computational complexity comes into the game. <br>
Apriori algorithm is based on very simple observation: **subsets of
frequent itemsets are also frequent itemsets**. In other words , if some
itemset is proven to be non-frequent , then it will not be considered by
algorithm any more for forming new frequent itemsets. To identify the
k-itemsets that are not frequent algorithm need to examine all subsets
of size (k-1) of each candidate k-itemset.<br> It generates candidate
itemsets of length k from item sets of length k-1. Then it prunes the
candidates which have an infrequent sub-pattern.

    rules <- apriori(data = subset(crimeData,year >= 2013)[,-c(1,2,3,6,7,8,9,10,11,14)] , 
    parameter = list(support = 0.03 , confidence = 0.6,maxlen = 5,target = 'rules'))

    ## Warning: Column(s) 1, 5 not logical or factor. Applying default
    ## discretization (see '? discretizeDF').

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##         0.6    0.1    1 none FALSE            TRUE       5    0.03      1
    ##  maxlen target   ext
    ##       5  rules FALSE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 31353 
    ## 
    ## set item appearances ...[0 item(s)] done [0.00s].
    ## set transactions ...[24 item(s), 1045101 transaction(s)] done [1.15s].
    ## sorting and recoding items ... [21 item(s)] done [0.10s].
    ## creating transaction tree ... done [2.75s].
    ## checking subsets of size 1 2 3 4 done [0.00s].
    ## writing ... [7 rule(s)] done [0.00s].
    ## creating S4 object  ... done [0.27s].

    inspect(sort(rules,by='lift'))

    ##     lhs                                  rhs                           support confidence     lift  count
    ## [1] {offenseLevel=VIOLATION}          => {occurenceLocation=INSIDE} 0.08360436  0.6466426 1.236093  87375
    ## [2] {time=[11.3,17.5),                                                                                   
    ##      Borough=MANHATTAN}               => {occurenceLocation=INSIDE} 0.05160458  0.6279414 1.200345  53932
    ## [3] {Borough=MANHATTAN,                                                                                  
    ##      offenseLevel=FELONY}             => {occurenceLocation=INSIDE} 0.04407995  0.6062536 1.158888  46068
    ## [4] {Borough=BRONX,                                                                                      
    ##      occurenceLocation=MISSING_VALUE} => {offenseLevel=MISDEMEANOR} 0.03050040  0.6278635 1.122953  31876
    ## [5] {time=[17.5,24],                                                                                     
    ##      Borough=BRONX}                   => {offenseLevel=MISDEMEANOR} 0.04869864  0.6273884 1.122103  50895
    ## [6] {time=[0,11.3),                                                                                      
    ##      occurenceLocation=MISSING_VALUE} => {offenseLevel=MISDEMEANOR} 0.04490858  0.6237491 1.115594  46934
    ## [7] {occurenceLocation=MISSING_VALUE} => {offenseLevel=MISDEMEANOR} 0.12501663  0.6005801 1.074156 130655

In the example above, the first couple of rules have the lift that is
slightly greater than 1 which means there might be light correlation
between these itemsets. On the other hand, this might be because value
"INSIDE" (1st rule) for occurenceLocation is dominating over the other
values of occurenceLocation. <br> Intuitive way of interpreting this
rule is something like "when crime belongs to the level VIOLATION, it is
slightly more likely that it happened INSIDE than then somewhere else".
<br> However, confidence of this rule could be somewhat better so we
can't accept that this is strong connection between these 2 variables
although lift implies some dependence.

### Trying to detect what is the cause of rare events

From summary table it is clear that most crimes have value COMPLETED for
category crimeCompleted, much less number of crimes are registered as
just ATTEMPTED. Association rules could allow us to find some specific
moments that imply this rare events. <br> Although lift is really high
for these events, their count is small(2-3) and these are not indicators
of any kind of correlation with ATTEMPTED value.

    rules <- apriori(data = subset(crimeData,year >= 2011)[,c(4,5,9,11,14)] , 
    parameter = list(support = 0.000001 , confidence = 0.85,maxlen = 5),
    appearance = list(rhs = c('crimeCompleted=ATTEMPTED')))

    ## Warning: Column(s) 1 not logical or factor. Applying default discretization
    ## (see '? discretizeDF').

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##        0.85    0.1    1 none FALSE            TRUE       5   1e-06      1
    ##  maxlen target   ext
    ##       5  rules FALSE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 1 
    ## 
    ## set item appearances ...[1 item(s)] done [0.00s].
    ## set transactions ...[150 item(s), 1046908 transaction(s)] done [1.20s].
    ## sorting and recoding items ... [146 item(s)] done [0.10s].
    ## creating transaction tree ... done [3.12s].
    ## checking subsets of size 1 2 3 4 5 done [0.05s].
    ## writing ... [10 rule(s)] done [0.01s].
    ## creating S4 object  ... done [0.33s].

    inspect(head(sort(rules,by='lift'),10))

    ##      lhs                                                     rhs                             support confidence     lift count
    ## [1]  {offenseDescription=FRAUDULENT_ACCOSTING,                                                                                
    ##       premiseDescription=PARKING_LOT_GARAGE_(PUBLIC)}     => {crimeCompleted=ATTEMPTED} 1.910388e-06          1 55.93055     2
    ## [2]  {offenseDescription=RAPE,                                                                                                
    ##       premiseDescription=BEAUTY_&_NAIL_SALON}             => {crimeCompleted=ATTEMPTED} 1.910388e-06          1 55.93055     2
    ## [3]  {time=[11.3,17.5),                                                                                                       
    ##       offenseDescription=RAPE,                                                                                                
    ##       premiseDescription=TRANSIT_NYC_SUBWAY}              => {crimeCompleted=ATTEMPTED} 2.865581e-06          1 55.93055     3
    ## [4]  {Borough=STATEN_ISLAND,                                                                                                  
    ##       offenseDescription=BURGLARY,                                                                                            
    ##       premiseDescription=BANK}                            => {crimeCompleted=ATTEMPTED} 1.910388e-06          1 55.93055     2
    ## [5]  {time=[11.3,17.5),                                                                                                       
    ##       Borough=MANHATTAN,                                                                                                      
    ##       offenseDescription=KIDNAPPING_AND_RELATED_OFFENSES,                                                                     
    ##       premiseDescription=STREET}                          => {crimeCompleted=ATTEMPTED} 1.910388e-06          1 55.93055     2
    ## [6]  {time=[0,11.3),                                                                                                          
    ##       Borough=MANHATTAN,                                                                                                      
    ##       offenseDescription=ROBBERY,                                                                                             
    ##       premiseDescription=BUS_STOP}                        => {crimeCompleted=ATTEMPTED} 1.910388e-06          1 55.93055     2
    ## [7]  {time=[17.5,24],                                                                                                         
    ##       Borough=MANHATTAN,                                                                                                      
    ##       offenseDescription=ROBBERY,                                                                                             
    ##       premiseDescription=BUS_STOP}                        => {crimeCompleted=ATTEMPTED} 2.865581e-06          1 55.93055     3
    ## [8]  {time=[11.3,17.5),                                                                                                       
    ##       Borough=MANHATTAN,                                                                                                      
    ##       offenseDescription=RAPE,                                                                                                
    ##       premiseDescription=TRANSIT_NYC_SUBWAY}              => {crimeCompleted=ATTEMPTED} 1.910388e-06          1 55.93055     2
    ## [9]  {time=[0,11.3),                                                                                                          
    ##       Borough=STATEN_ISLAND,                                                                                                  
    ##       offenseDescription=BURGLARY,                                                                                            
    ##       premiseDescription=BANK}                            => {crimeCompleted=ATTEMPTED} 1.910388e-06          1 55.93055     2
    ## [10] {time=[11.3,17.5),                                                                                                       
    ##       Borough=STATEN_ISLAND,                                                                                                  
    ##       offenseDescription=BURGLARY,                                                                                            
    ##       premiseDescription=SMALL_MERCHANT}                  => {crimeCompleted=ATTEMPTED} 1.910388e-06          1 55.93055     2

Greater count implies that we need to sacrifice confidence. <br>
Left-hand side of these rules with great lift value, contains some
specific events like explicit part of the day when
"KIDNAPPING\_AND\_RELATED\_OFFENSES" crimes happend on the street.

    rules <- apriori(data = subset(crimeData,year >= 2013)[,c(4,5,9,11,14)] , 
    parameter = list(support = 0.00001 , confidence = 0.55,maxlen = 5),
    appearance = list(rhs = c('crimeCompleted=ATTEMPTED')))

    ## Warning: Column(s) 1 not logical or factor. Applying default discretization
    ## (see '? discretizeDF').

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##        0.55    0.1    1 none FALSE            TRUE       5   1e-05      1
    ##  maxlen target   ext
    ##       5  rules FALSE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 10 
    ## 
    ## set item appearances ...[1 item(s)] done [0.00s].
    ## set transactions ...[150 item(s), 1045101 transaction(s)] done [1.22s].
    ## sorting and recoding items ... [135 item(s)] done [0.12s].
    ## creating transaction tree ... done [3.15s].
    ## checking subsets of size 1 2 3 4 5 done [0.04s].
    ## writing ... [3 rule(s)] done [0.00s].
    ## creating S4 object  ... done [0.32s].

    inspect(head(sort(rules,by='lift'),10))

    ##     lhs                                                     rhs                             support confidence     lift count
    ## [1] {time=[11.3,17.5),                                                                                                       
    ##      offenseDescription=KIDNAPPING_AND_RELATED_OFFENSES,                                                                     
    ##      premiseDescription=STREET}                          => {crimeCompleted=ATTEMPTED} 1.435268e-05  0.6521739 36.47975    15
    ## [2] {Borough=BRONX,                                                                                                          
    ##      offenseDescription=ROBBERY,                                                                                             
    ##      premiseDescription=CHECK_CASHING_BUSINESS}          => {crimeCompleted=ATTEMPTED} 1.339583e-05  0.5833333 32.62911    14
    ## [3] {time=[0,11.3),                                                                                                          
    ##      offenseDescription=ROBBERY,                                                                                             
    ##      premiseDescription=CHECK_CASHING_BUSINESS}          => {crimeCompleted=ATTEMPTED} 1.913691e-05  0.5555556 31.07534    20

Association rules allow us to discover nature of serious crimes, like
burglary and larceny (for 2015 year). <br> Rules with higher lift and
confidence are good candidates for better research because they imply
that there might be some connection between certain variables in this
subset of data.

    rules <- apriori(data = subset(crimeData,year == 2015)[,c(4,5,9,11,14,15)] , 
    parameter = list(support = 0.00001 , confidence = 0.7,maxlen = 5,target='rules'),
    appearance = list(rhs = c('offenseDescription=BURGLARY')))

    ## Warning: Column(s) 1, 6 not logical or factor. Applying default
    ## discretization (see '? discretizeDF').

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##         0.7    0.1    1 none FALSE            TRUE       5   1e-05      1
    ##  maxlen target   ext
    ##       5  rules FALSE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 4 
    ## 
    ## set item appearances ...[1 item(s)] done [0.00s].
    ## set transactions ...[154 item(s), 468576 transaction(s)] done [0.60s].
    ## sorting and recoding items ... [143 item(s)] done [0.06s].
    ## creating transaction tree ... done [1.41s].
    ## checking subsets of size 1 2 3 4 5

    ## Warning in apriori(data = subset(crimeData, year == 2015)[, c(4, 5, 9,
    ## 11, : Mining stopped (maxlen reached). Only patterns up to a length of 5
    ## returned!

    ##  done [0.17s].
    ## writing ... [12 rule(s)] done [0.01s].
    ## creating S4 object  ... done [0.14s].

    inspect(head(sort(rules,by='count'),10))

    ##      lhs                                       rhs                                support confidence     lift count
    ## [1]  {time=[11.3,17.5),                                                                                            
    ##       Borough=BRONX,                                                                                               
    ##       premiseDescription=CONSTRUCTION_SITE,                                                                        
    ##       weekDay=Friday}                       => {offenseDescription=BURGLARY} 2.987776e-05  0.8235294 25.72574    14
    ## [2]  {time=[0,11.3),                                                                                               
    ##       Borough=QUEENS,                                                                                              
    ##       crimeCompleted=ATTEMPTED,                                                                                    
    ##       premiseDescription=RESTAURANT_DINER}  => {offenseDescription=BURGLARY} 2.560951e-05  0.7500000 23.42880    12
    ## [3]  {time=[17.5,24],                                                                                              
    ##       Borough=BROOKLYN,                                                                                            
    ##       premiseDescription=CONSTRUCTION_SITE,                                                                        
    ##       weekDay=Thursday}                     => {offenseDescription=BURGLARY} 1.920713e-05  0.7500000 23.42880     9
    ## [4]  {time=[0,11.3),                                                                                               
    ##       Borough=BROOKLYN,                                                                                            
    ##       crimeCompleted=ATTEMPTED,                                                                                    
    ##       premiseDescription=RESTAURANT_DINER}  => {offenseDescription=BURGLARY} 1.920713e-05  0.7500000 23.42880     9
    ## [5]  {time=[11.3,17.5),                                                                                            
    ##       Borough=BRONX,                                                                                               
    ##       premiseDescription=CONSTRUCTION_SITE,                                                                        
    ##       weekDay=Saturday}                     => {offenseDescription=BURGLARY} 1.707300e-05  0.8000000 24.99072     8
    ## [6]  {time=[11.3,17.5),                                                                                            
    ##       Borough=BROOKLYN,                                                                                            
    ##       premiseDescription=CONSTRUCTION_SITE,                                                                        
    ##       weekDay=Sunday}                       => {offenseDescription=BURGLARY} 1.280475e-05  0.7500000 23.42880     6
    ## [7]  {time=[0,11.3),                                                                                               
    ##       crimeCompleted=ATTEMPTED,                                                                                    
    ##       premiseDescription=RESTAURANT_DINER,                                                                         
    ##       weekDay=Monday}                       => {offenseDescription=BURGLARY} 1.280475e-05  0.8571429 26.77577     6
    ## [8]  {time=[0,11.3),                                                                                               
    ##       crimeCompleted=ATTEMPTED,                                                                                    
    ##       premiseDescription=RESTAURANT_DINER,                                                                         
    ##       weekDay=Saturday}                     => {offenseDescription=BURGLARY} 1.280475e-05  0.7500000 23.42880     6
    ## [9]  {crimeCompleted=ATTEMPTED,                                                                                    
    ##       premiseDescription=CHURCH,                                                                                   
    ##       weekDay=Saturday}                     => {offenseDescription=BURGLARY} 1.067063e-05  0.8333333 26.03200     5
    ## [10] {Borough=QUEENS,                                                                                              
    ##       crimeCompleted=ATTEMPTED,                                                                                    
    ##       premiseDescription=CHURCH}            => {offenseDescription=BURGLARY} 1.067063e-05  1.0000000 31.23840     5

    rules <- apriori(data = subset(crimeData,year == 2015)[,c(4,5,9,11,14,15)] , 
    parameter = list(support = 0.00001 , confidence = 0.75,maxlen = 5,target='rules'),
    appearance = list(rhs = c('offenseDescription=GRAND_LARCENY')))

    ## Warning: Column(s) 1, 6 not logical or factor. Applying default
    ## discretization (see '? discretizeDF').

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##        0.75    0.1    1 none FALSE            TRUE       5   1e-05      1
    ##  maxlen target   ext
    ##       5  rules FALSE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 4 
    ## 
    ## set item appearances ...[1 item(s)] done [0.00s].
    ## set transactions ...[154 item(s), 468576 transaction(s)] done [1.02s].
    ## sorting and recoding items ... [143 item(s)] done [0.09s].
    ## creating transaction tree ... done [2.22s].
    ## checking subsets of size 1 2 3 4 5

    ## Warning in apriori(data = subset(crimeData, year == 2015)[, c(4, 5, 9,
    ## 11, : Mining stopped (maxlen reached). Only patterns up to a length of 5
    ## returned!

    ##  done [0.21s].
    ## writing ... [26 rule(s)] done [0.01s].
    ## creating S4 object  ... done [0.18s].

    inspect(head(sort(rules,by='count'),10))

    ##      lhs                           rhs                                     support confidence      lift count
    ## [1]  {time=[0,11.3),                                                                                         
    ##       Borough=QUEENS,                                                                                        
    ##       premiseDescription=ATM}   => {offenseDescription=GRAND_LARCENY} 4.695076e-05  0.8461538  9.425140    22
    ## [2]  {time=[0,11.3),                                                                                         
    ##       Borough=QUEENS,                                                                                        
    ##       crimeCompleted=COMPLETED,                                                                              
    ##       premiseDescription=ATM}   => {offenseDescription=GRAND_LARCENY} 4.268251e-05  0.8333333  9.282335    20
    ## [3]  {time=[0,11.3),                                                                                         
    ##       premiseDescription=ATM,                                                                                
    ##       weekDay=Thursday}         => {offenseDescription=GRAND_LARCENY} 3.628013e-05  0.7727273  8.607256    17
    ## [4]  {time=[0,11.3),                                                                                         
    ##       crimeCompleted=COMPLETED,                                                                              
    ##       premiseDescription=ATM,                                                                                
    ##       weekDay=Thursday}         => {offenseDescription=GRAND_LARCENY} 3.414601e-05  0.7619048  8.486707    16
    ## [5]  {time=[0,11.3),                                                                                         
    ##       premiseDescription=ATM,                                                                                
    ##       weekDay=Wednesday}        => {offenseDescription=GRAND_LARCENY} 3.201188e-05  0.7500000  8.354102    15
    ## [6]  {crimeCompleted=ATTEMPTED,                                                                              
    ##       premiseDescription=ATM}   => {offenseDescription=GRAND_LARCENY} 2.774363e-05  0.7647059  8.517908    13
    ## [7]  {time=[0,11.3),                                                                                         
    ##       crimeCompleted=ATTEMPTED,                                                                              
    ##       premiseDescription=ATM}   => {offenseDescription=GRAND_LARCENY} 1.920713e-05  1.0000000 11.138802     9
    ## [8]  {Borough=QUEENS,                                                                                        
    ##       premiseDescription=ATM,                                                                                
    ##       weekDay=Friday}           => {offenseDescription=GRAND_LARCENY} 1.920713e-05  0.7500000  8.354102     9
    ## [9]  {Borough=QUEENS,                                                                                        
    ##       crimeCompleted=COMPLETED,                                                                              
    ##       premiseDescription=ATM,                                                                                
    ##       weekDay=Friday}           => {offenseDescription=GRAND_LARCENY} 1.920713e-05  0.7500000  8.354102     9
    ## [10] {Borough=QUEENS,                                                                                        
    ##       premiseDescription=ATM,                                                                                
    ##       weekDay=Thursday}         => {offenseDescription=GRAND_LARCENY} 1.707300e-05  0.8888889  9.901158     8

Hotspots detection
------------------

Crime hotspots are parts of the city that have high crime intensity.
<br> Motivation for crime hotspot analysis is detecting crime hotspots
for focusing police forces on potential residence of criminal activity.
Analyzing crime locations and information associated with them is one of
fundamental tasks of every crime analysis. This kind of analysis is
significant bacause it can easily reveal that risk of being a victim of
certain type of crime is not always geographically constant. <br> Crime
pattern theory implies that generally crime is not random. The first
definition suggests that clustering crimes based on density should be
right way to go.

    library(dbscan)
    library(ggmap)

    ## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.

    ## Please cite ggmap if you use it! See citation("ggmap") for details.

    library(leaflet)
    # Citation:
    citation("ggmap")

    ## 
    ## To cite ggmap in publications, please use:
    ## 
    ##   D. Kahle and H. Wickham. ggmap: Spatial Visualization with
    ##   ggplot2. The R Journal, 5(1), 144-161. URL
    ##   http://journal.r-project.org/archive/2013-1/kahle-wickham.pdf
    ## 
    ## A BibTeX entry for LaTeX users is
    ## 
    ##   @Article{,
    ##     author = {David Kahle and Hadley Wickham},
    ##     title = {ggmap: Spatial Visualization with ggplot2},
    ##     journal = {The R Journal},
    ##     year = {2013},
    ##     volume = {5},
    ##     number = {1},
    ##     pages = {144--161},
    ##     url = {https://journal.r-project.org/archive/2013-1/kahle-wickham.pdf},
    ##   }

DBSCAN algorithm
----------------

For purpose of detecting crime hotspots it is appropriate to use
DBSCAN(density-based spatial clustering of applications with noise)
clustering algorithm. For a given a set of points in space, it groups
together points that are closely packed together(nearby neigbors). <br>
Source: <https://en.wikipedia.org/wiki/DBSCAN>

### Simple example

Murders in Bronx in period 2014 and 2015

    data = subset(crimeData, year >= 2014 & Borough == "BRONX" & 
    offenseDescription == "MURDER_AND_NON_NEGL_MANSLAUGHTER" & !is.na(Longitude) &
    !is.na(Latitude))[,c(7,8)] 
    options(repr.plot.width=9, repr.plot.height=7)
    leaflet() %>% addTiles() %>% addCircleMarkers(lng=data$Longitude, lat=data$Latitude)

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-23-1.png)

    options(repr.plot.width=8, repr.plot.height=7) # possible hotspot
    clust = dbscan(x = data,eps = 0.01,minPts = 20,borderPoints = F)
    leaflet() %>% addTiles() %>% 
    addCircleMarkers(lng=data$Longitude[which(clust$cluster==1)], lat=data$Latitude[which(clust$cluster==1)])

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-24-1.png)

### Robberies in Queens (2015)

Although crime hotspots can be found relatively easy with DBSCAN, they
might be very natural because of greater density of population in that
places(not visible from this data). <br> Great density of population
might imply greater density of some specific crime level.

    options(repr.plot.width=8, repr.plot.height=8)
    data <- subset(crimeData, year >= 2015 & month>=10 & Borough == "QUEENS" & 
    offenseDescription == "ROBBERY" & !is.na(Longitude) & !is.na(Latitude))[,c(7,8)]
    leaflet() %>% addTiles() %>% addCircleMarkers(lng=data$Longitude, lat=data$Latitude)

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-25-1.png)

    clust = dbscan(x = data,eps = 0.0095,minPts = 35,borderPoints = F)
    leaflet() %>% addTiles() %>% 
    addCircleMarkers(lng=data$Longitude[which(clust$cluster>=1)], lat=data$Latitude[which(clust$cluster>=1)])

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-26-1.png)

In example above DBSCAN algorithm found few clusters that could
represent possible hotspots for certain level of crime. <br> However,
there are other methods for searching hotspots, like test for
clustering. Testing for clustering is the first step in revealing
whether data has crime hotspots.

Nearest neighbor index (NNI)
----------------------------

NNI is a very simple and quick method to test evidence of clustering.
<br> This test compares the actual distribution of crime data against a
data set of the same size, but with random distribution. <br> The test
has following steps: <br> First, calculate observed average nearest
neighbor distance (for every point, find it's closest neighbor and
calculate their distance, then average all those distances). <br> Do the
same thing for random distribution of the same size - average random
nearest neighbor distance. NNI is the ratio of the observed average
nearest neighbor distance against the average random nearest neighbor
distance.<br> If the result generated from the NNI test is 1, then the
crime data are randomly distributed.<br> If the NNI is less than 1, then
the crime data show some evidence of clustering.<br> An NNI that is
greater than 1 reveals evidence of a uniform pattern in crime data.

Z-score test statistics
-----------------------

A z-score test statistic can be applied to gain confidence in the NNI
result. This test of statistical significance describes how different
the actual average nearest neighbor distance is from the average random
nearest neighbor distance. <br> General principle is that the more
negative the z-score, the more confidence can be placed in the NNI
result.

### Example

In the example above it is easy to find NNI which is about 0.62 which
means this should be the evidence that clustering is not just a
coincidence.

    library(spatialEco) # library for NNI statistics
    library(sp)

    subset(crimeData, year >= 2015 & month>=10 & Borough == "QUEENS" & 
    offenseDescription == "ROBBERY" & !is.na(Longitude) & !is.na(Latitude))[,c(7,8)] %>%
    SpatialPoints() %>% nni(win = 'hull')

    ## Warning: data contain duplicated points

    ## $NNI
    ## [1] 0.6287652
    ## 
    ## $z.score
    ## [1] -20.62022
    ## 
    ## $p
    ## [1] 1.80734e-94
    ## 
    ## $expected.mean.distance
    ## [1] 0.003302649
    ## 
    ## $observed.mean.distance
    ## [1] 0.002076591

Crime oportunity - vehicle crimes
---------------------------------

Some places have great oportunity for vehicle crimes

    options(repr.plot.width=8, repr.plot.height=5)
    data = subset(crimeData,year >= 2014 & Borough == "STATEN_ISLAND" & offenseDescription == "VEHICLE_AND_TRAFFIC_LAWS" & 
    !is.na(Longitude) & !is.na(Latitude))[,c(7,8)]
    leaflet() %>% addTiles() %>% addCircleMarkers(lng=data$Longitude, lat=data$Latitude)

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-29-1.png)

    clust = dbscan(x = data,eps = 0.02,minPts = 45,borderPoints = F)
    leaflet() %>% addTiles() %>% addCircleMarkers(lng=data$Longitude[which(clust$cluster>=1)], lat=data$Latitude[which(clust$cluster>=1)])

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-30-1.png)

    nni(SpatialPoints(data))

    ## Warning: data contain duplicated points

    ## $NNI
    ## [1] 0.648319
    ## 
    ## $z.score
    ## [1] -11.94084
    ## 
    ## $p
    ## [1] 7.248425e-33
    ## 
    ## $expected.mean.distance
    ## [1] 0.003290331
    ## 
    ## $observed.mean.distance
    ## [1] 0.002133184

Dangerous drugs hotspots
------------------------

Hotspot analysis can be very simple and suitable method for prevention
of selling drugs. <br> It might be the answer on the question "Where the
most amount of drugs are being sold or consumed?".

    options(repr.plot.width=8, repr.plot.height=8)
    data = subset(crimeData, year >= 2015 & month>=10 & Borough == "BRONX" & 
    offenseDescription == "DANGEROUS_DRUGS" & !is.na(Longitude) & !is.na(Latitude))[,c(7,8)]
    leaflet() %>% addTiles() %>% addCircleMarkers(lng=data$Longitude, lat=data$Latitude)

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-31-1.png)

    clust = dbscan(x = data,eps = 0.0035,minPts = 60,borderPoints = F)
    leaflet() %>% addTiles() %>% addCircleMarkers(lng=data$Longitude[which(clust$cluster>=1)], lat=data$Latitude[which(clust$cluster>=1)])

![](Crime_analysis_project_files/figure-markdown_strict/unnamed-chunk-32-1.png)

    nni(SpatialPoints(data))

    ## Warning: data contain duplicated points

    ## $NNI
    ## [1] 0.4859611
    ## 
    ## $z.score
    ## [1] -44.8927
    ## 
    ## $p
    ## [1] 0
    ## 
    ## $expected.mean.distance
    ## [1] 0.001118357
    ## 
    ## $observed.mean.distance
    ## [1] 0.0005434781

Conclusion
----------

Crime is one of inevitable parts of today's civilization, especially in
greater areas and cities. In last few decades, due to development of new
tehnologies and numerous number of statistical researches, scientists
and researchers developed advanced statistical concepts that are very
useful for crime analysis and prevention. <br> Through the years these
analyses showed that generally crime is not random, there are certain
causes of these events that can be understood and predicted to some
measure. <br> Even simple statisical analysis and tests can reveal
correlations in data that are not visible and obvious at first
sight.<br> The question that rises up after any kind of crime analysis
and prevention is, will those attempts of prevention stop some amount of
crime to happen in some places, but cause the same type of crimes to
occur in different places.<br> For example, hotspot analysis can reveal
drug-hotspots and police actually manage to repress that kind of crimes,
but after a while crimes related to drugs start to occur in some other
places, forming new hotspots that did not exist before.
