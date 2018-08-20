---
title: "STA 380 Homework 2"
author: "Aifaz Gowani, Jonathan Evans, Pooshan Shah"
output:
  html_document:
    keep_md: yes
  pdf_document: default
---
## QUESTION 1
## AIRPORT ANALYSIS 

Question #1: Your task is to create a figure, or set of related figures, that tell an interesting story about flights into and out of Austin. You can annotate the figure and briefly describe it, but strive to make it as stand-alone as possible. It shouldn't need many, many paragraphs to convey its meaning. Rather, the figure should speak for itself as far as possible.


```r
airports <- read.csv("ABIA.csv", header = TRUE)
attach(airports)
plot(DepTime,DepDelay, xlab='Dep Time: Hour-minute format',ylab='Dep Delay: minutes',col='indianred1')
```

![](STA_380_Homework_2_Aifaz_Gowani_Jonathan_Evans_Pooshan_Shah_files/figure-html/unnamed-chunk-1-1.png)<!-- -->
First, we wanted to see whether there is any pattern between the time of the day and by how many minutes are the flights are delayed. As you can see there is pattern that emerges where the flights that are closer to midnight, tend to be delayed more than those that are around 8 o'clock in the morning. 

Another interesting thing was the how come there are not really any domestic flights between roughly from 1 to 5 A.M? Upon doing some research I found out that this is the case because the pilots need rest, less taxis are available, less facilities at the airport are available etc. 

An example of the Taxis pattern is also shown below which supports the same pattern: 

```r
plot(DepTime,TaxiOut,col='orange',xlab= 'Dep Time: Hour-minute format',ylab='Taxi Out')
```

![](STA_380_Homework_2_Aifaz_Gowani_Jonathan_Evans_Pooshan_Shah_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

Second, we wanted to identify which months are the best to fly in terms of less departure delay time and which months tend to have the lowest delays in departure times. From the graph below it could be concluded that the best month is september with the lowest delay time and worst comes out to be july and december. we plotted out the summaries for these dates so it can be compared in numerical terms. 


```r
plot(Month,DepDelay, xlab='Month',ylab='Dep Delay: minutes',col='blue')
```

![](STA_380_Homework_2_Aifaz_Gowani_Jonathan_Evans_Pooshan_Shah_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
Numerical comparison via summaries is as follows: 

```r
sept_only = subset(airports, Month==9)
summary(sept_only$DepDelay)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
## -20.000  -5.000  -2.000   3.327   1.000 272.000     154
```

```r
dec_only = subset(airports,Month ==12)
summary(dec_only$DepDelay)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##  -16.00   -3.00    1.00   15.34   15.25  875.00      84
```

```r
july_only  =subset(airports, Month==7)
summary(july_only$DepDelay)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##  -42.00   -4.00    0.00   10.23    7.00  665.00      90
```

You can clearly see that the median and mean are much lower for the month of september in comparison to the other two months. This makes sense because July and December are holiday months typically therefore there is an increase in the delay time as well . 

Another graph to look at for this comparison is presented below: 

```r
boxplot(sept_only$DepDelay,(dec_only$DepDelay), xlab ='September                                                                            December',ylab='Dep Delay: minutes',main='Dep Delay comparsion between Sept and Dec',col='red')
```

![](STA_380_Homework_2_Aifaz_Gowani_Jonathan_Evans_Pooshan_Shah_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

Third, we wanted to explore how destination varies depending on the time of the month. As you can see it in the boxplot presented below, a lot of California and Nevada Locations(Las Vegas) are particularly popular in the summer times. While there patterns which can also be seen such as Ontario during the month of January because people may want to go their for winter break as well. 


```r
plot(Dest,Month, xlab='Destination',ylab='Time of the month',col='green')
```

![](STA_380_Homework_2_Aifaz_Gowani_Jonathan_Evans_Pooshan_Shah_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

Fourth, If you hate flight delays, then what are the airports that you should avoid. Surprisingly, Austin's airport came out to be among the worst however this could be the lack of data that was available. Please refer to the graph below for a graphical presentation: 


```r
knitr::opts_chunk$set(fig.width=12, fig.height=8, fig.path='Figs/',
                      echo=FALSE, warning=FALSE, message=FALSE)
plot(Dest,log(ArrDelay), xlab='Dest',ylab='Dep Delay: log(minutes)',col='yellow')
```

![](STA_380_Homework_2_Aifaz_Gowani_Jonathan_Evans_Pooshan_Shah_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


Fifth, we wanted to know what are the possible combinations of airports that you can fly to. In other words, we wnated to identify where are the large airports located and how many airports are located along the east and the west side of the United States. In order to see this, we plotted it on a map using ggmap library. 

![](Figs/unnamed-chunk-8-1.png)<!-- -->


Lastly, we wanted to identify what are the possible location that you can go to from the Austin's airport. Below is a graphical representation that shows the large capacity of Austin's airport to accomodate many different locations around United States and more. This also points out that because there are so many locations that you can go from Austin, it increases the complexity of time management for the air control crew and as a result cause some delays in the flights at the Austin's airport as we saw in the boxplot before. 

![](Figs/unnamed-chunk-9-1.png)<!-- -->



## QUESTION 2
## Author Attribution

Revisit the Reuters C50 corpus that we explored in class. Your task is to build two separate models (using any combination of tools you see fit) for predicting the author of an article on the basis of that article's textual content. Describe clearly what models you are using, how you constructed features, and so forth. Yes, this is a supervised learning task, but it potentially draws on a lot of what you know about unsupervised learning, since constructing features for a document might involve dimensionality reduction.

In the C50train directory, you have ~50 articles from each of 50 different authors (one author per directory). Use this training data (and this data alone) to build the two models. Then apply your model to the articles by the same authors in the C50test directory, which is about the same size as the training set. How well do your models do at predicting the author identities in this out-of-sample setting? Are there any sets of authors whose articles seem difficult to distinguish from one another? Which model do you prefer?





_______________________________________________

Basic statistics about the Train Data:

```
## <<DocumentTermMatrix (documents: 2500, terms: 801)>>
## Non-/sparse entries: 280686/1721814
## Sparsity           : 86%
## Maximal term length: 18
## Weighting          : term frequency (tf)
```

We see here that we've reduced the number of terms down to 801 terms with a sparsity of 86%.

_______________________________________________



_______________________________________________

Basic statistics about the Test Data:

```
## <<DocumentTermMatrix (documents: 2500, terms: 816)>>
## Non-/sparse entries: 285048/1754952
## Sparsity           : 86%
## Maximal term length: 18
## Weighting          : term frequency (tf)
```

We see here that we've reduced the number of terms down to 816 terms with a sparsity of 86%.

_______________________________________________

Differences between training and testing set:

```
##    Mode   FALSE    TRUE 
## logical      74     742
```

There are 74 terms that are not shared between the sets. From the problem statement, we want to add "pseudo-words" to include unseen terms so that they don't give a 0% probability. 

_______________________________________________

Adding in pseudo-word counts to account for the unseen words:

```
##    Mode    TRUE 
## logical     801
```

Now all words are shared between the 2 sets of train and test.

_______________________________________________



Here we transformed the data sets in to matrices and data frames to be used in the modeling algorithms. We then binded the authors of each document to the documents as a way to include the y-variable in the data frame. 

_______________________________________________

Models used: 
1) Naive Bayes
2) Random Forest

### Naive Bayes

Accuracy of the Naive Bayes 

```
## Accuracy 
##   0.2828
```

Naive Bayes gets an accuracy of 28.28%

_______________________________________________

Top 20 Authors who were correctly distinguished:

```
##                 Var1    nB_predictions Freq
## 1      PeterHumphrey     PeterHumphrey   45
## 2       RogerFillion      RogerFillion   44
## 3          LydiaZajc         LydiaZajc   39
## 4  KouroshKarimkhany KouroshKarimkhany   38
## 5         AlanCrosby        AlanCrosby   36
## 6      AaronPressman     AaronPressman   33
## 7       JimGilchrist      JimGilchrist   33
## 8         RobinSidel        RobinSidel   33
## 9     LynneO'Donnell    LynneO'Donnell   32
## 10      MatthewBunce      MatthewBunce   29
## 11    FumikoFujisaki    FumikoFujisaki   26
## 12   BenjaminKangLim   BenjaminKangLim   25
## 13    TheresePoletti    TheresePoletti   25
## 14        TimFarrand        TimFarrand   22
## 15       DavidLawder       DavidLawder   21
## 16    JoWinterbottom    JoWinterbottom   21
## 17    GrahamEarnshaw    GrahamEarnshaw   16
## 18        PierreTran        PierreTran   15
## 19     BernardHickey     BernardHickey   14
## 20       KarlPenhaul       KarlPenhaul   13
```

_______________________________________________

Top 20 Authors who were difficult to distinguish from other authors:

```
##               Var1    nB_predictions Freq
## 1         TanEeLyn     PeterHumphrey   37
## 2       ToddNissen       DavidLawder   32
## 3     SarahDavison     PeterHumphrey   25
## 4       MureDickie   BenjaminKangLim   23
## 5      ScottHillis     PeterHumphrey   23
## 6    JaneMacartney     PeterHumphrey   21
## 7      ScottHillis   BenjaminKangLim   20
## 8  PatriciaCommins KouroshKarimkhany   19
## 9     WilliamKazer     PeterHumphrey   19
## 10   JaneMacartney   BenjaminKangLim   17
## 11 LynnleyBrowning       DavidLawder   17
## 12      JanLopatka        AlanCrosby   16
## 13      MartinWolk KouroshKarimkhany   16
## 14     SamuelPerry KouroshKarimkhany   16
## 15    WilliamKazer   BenjaminKangLim   15
## 16   EdnaFernandes       DavidLawder   15
## 17  KevinDrawbaugh       DavidLawder   15
## 18     BradDorfman       DavidLawder   14
## 19     KarlPenhaul     PeterHumphrey   14
## 20      MureDickie     PeterHumphrey   14
```

_______________________________________________

### Random Forest

Accuracy of Random Forest:

```
## Accuracy 
##   0.7604
```

Random Forest gets an accuracy of 76.04%

_______________________________________________

Top 20 authors who were correctly distinguished:

```
##              rf.pred     train_authors Freq
## 1       JimGilchrist      JimGilchrist   49
## 2     LynneO'Donnell    LynneO'Donnell   49
## 3     FumikoFujisaki    FumikoFujisaki   48
## 4       MatthewBunce      MatthewBunce   47
## 5  KouroshKarimkhany KouroshKarimkhany   46
## 6    MarcelMichelson   MarcelMichelson   46
## 7      AaronPressman     AaronPressman   45
## 8         AlanCrosby        AlanCrosby   45
## 9          LydiaZajc         LydiaZajc   45
## 10       DavidLawder       DavidLawder   44
## 11       KarlPenhaul       KarlPenhaul   44
## 12      RogerFillion      RogerFillion   44
## 13    GrahamEarnshaw    GrahamEarnshaw   43
## 14        JanLopatka        JanLopatka   43
## 15     PeterHumphrey     PeterHumphrey   43
## 16  DarrenSchuettler  DarrenSchuettler   42
## 17    JoWinterbottom    JoWinterbottom   42
## 18   LynnleyBrowning   LynnleyBrowning   42
## 19  HeatherScoffield  HeatherScoffield   41
## 20         NickLouth         NickLouth   41
```

_______________________________________________

Top 20 Authors who were difficult to distinguish from other authors:

```
##            rf.pred train_authors Freq
## 1  BenjaminKangLim    MureDickie   14
## 2  BenjaminKangLim JaneMacartney   12
## 3  BenjaminKangLim   ScottHillis   12
## 4       JanLopatka  JohnMastrini    9
## 5    BernardHickey KevinMorrison    8
## 6      ScottHillis JaneMacartney    7
## 7     JimGilchrist      TanEeLyn    7
## 8    PeterHumphrey      TanEeLyn    7
## 9   KevinDrawbaugh   BradDorfman    6
## 10     ScottHillis    MureDickie    6
## 11     EricAuchard   SamuelPerry    6
## 12        TanEeLyn  SarahDavison    6
## 13 BenjaminKangLim  WilliamKazer    6
## 14     ScottHillis  WilliamKazer    6
## 15    JimGilchrist KevinMorrison    5
## 16        TanEeLyn PeterHumphrey    5
## 17   AaronPressman  RogerFillion    5
## 18    JimGilchrist  SarahDavison    5
## 19      MureDickie   ScottHillis    5
## 20     DavidLawder    ToddNissen    5
```

_______________________________________________

Naive Bayes gets an accuracy of 28.28% and Random Forest gets an accuracy of 76.04% so we prefer to go with Random Forest in predicting authors based on the training set. In both models, we see that authors: Benjamin Kang Lim and Mure Dickie are very difficult to distinguish from each other. 
Naive Bayes had the hardest time distinguishing between TanEeLyn and Peter Humphrey and Random Forest had the hardest time distinguishing between Benjamin Kang Lim and Mure Dickie. 





## QUESTION 3
## Association Rule Mining







We created a stacked dataframe that puts all the items into one column. We then created ID numbers that are associated with each basket. This will be very useful later.

![](Figs/unnamed-chunk-26-1.png)<!-- -->

We wanted to get an idea of what the most popular items were. This may help with gathering insights later on when we look at networks and association rules for this data. As you can see, whole milk is the most popular item.


```
## Apriori
## 
## Parameter specification:
##  confidence minval smax arem  aval originalSupport maxtime support minlen
##       0.003    0.1    1 none FALSE            TRUE       5   9e-04      1
##  maxlen target   ext
##      10  rules FALSE
## 
## Algorithmic control:
##  filter tree heap memopt load sort verbose
##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
## 
## Absolute minimum support count: 8 
## 
## set item appearances ...[0 item(s)] done [0.00s].
## set transactions ...[169 item(s), 9835 transaction(s)] done [0.02s].
## sorting and recoding items ... [158 item(s)] done [0.00s].
## creating transaction tree ... done [0.00s].
## checking subsets of size 1 2 3 4 5 6 done [0.00s].
## writing ... [50370 rule(s)] done [0.02s].
## creating S4 object  ... done [0.01s].
```

```
##      lhs                        rhs                       support confidence      lift count
## [1]  {liquor,                                                                               
##       red/blush wine}        => {bottled beer}       0.0019318760  0.9047619 11.235269    19
## [2]  {citrus fruit,                                                                         
##       other vegetables,                                                                     
##       rice}                  => {root vegetables}    0.0009150991  0.8181818  7.506360     9
## [3]  {beverages,                                                                            
##       other vegetables,                                                                     
##       whipped/sour cream}    => {tropical fruit}     0.0009150991  0.8181818  7.797304     9
## [4]  {citrus fruit,                                                                         
##       fruit/vegetable juice,                                                                
##       grapes}                => {tropical fruit}     0.0011184545  0.8461538  8.063879    11
## [5]  {curd,                                                                                 
##       frozen meals,                                                                         
##       other vegetables}      => {root vegetables}    0.0009150991  0.8181818  7.506360     9
## [6]  {hard cheese,                                                                          
##       tropical fruit,                                                                       
##       whipped/sour cream}    => {butter}             0.0009150991  0.8181818 14.764804     9
## [7]  {frozen vegetables,                                                                    
##       other vegetables,                                                                     
##       sliced cheese}         => {root vegetables}    0.0009150991  0.8181818  7.506360     9
## [8]  {oil,                                                                                  
##       onions,                                                                               
##       whole milk}            => {root vegetables}    0.0009150991  0.9000000  8.256996     9
## [9]  {chicken,                                                                              
##       citrus fruit,                                                                         
##       frankfurter}           => {root vegetables}    0.0009150991  0.9000000  8.256996     9
## [10] {other vegetables,                                                                     
##       rice,                                                                                 
##       whole milk,                                                                           
##       yogurt}                => {root vegetables}    0.0013218099  0.8666667  7.951182    13
## [11] {butter,                                                                               
##       hard cheese,                                                                          
##       other vegetables,                                                                     
##       whole milk}            => {whipped/sour cream} 0.0009150991  0.8181818 11.413926     9
## [12] {domestic eggs,                                                                        
##       hard cheese,                                                                          
##       other vegetables,                                                                     
##       whole milk}            => {root vegetables}    0.0009150991  0.9000000  8.256996     9
## [13] {ham,                                                                                  
##       other vegetables,                                                                     
##       pip fruit,                                                                            
##       yogurt}                => {tropical fruit}     0.0010167768  0.8333333  7.941699    10
## [14] {citrus fruit,                                                                         
##       fruit/vegetable juice,                                                                
##       oil,                                                                                  
##       other vegetables}      => {root vegetables}    0.0009150991  0.8181818  7.506360     9
## [15] {oil,                                                                                  
##       other vegetables,                                                                     
##       tropical fruit,                                                                       
##       whole milk}            => {root vegetables}    0.0013218099  0.8666667  7.951182    13
```

```
## [1] 64
```

```
##     lhs                               rhs                support     
## [1] {rice,sugar}                   => {whole milk}       0.0012201322
## [2] {curd,frozen fish}             => {whole milk}       0.0009150991
## [3] {canned fish,hygiene articles} => {whole milk}       0.0011184545
## [4] {butter,rice,root vegetables}  => {whole milk}       0.0010167768
## [5] {brown bread,herbs,whole milk} => {other vegetables} 0.0009150991
##     confidence lift     count
## [1] 1          3.913649 12   
## [2] 1          3.913649  9   
## [3] 1          3.913649 11   
## [4] 1          3.913649 10   
## [5] 1          5.168156  9
```

![](Figs/unnamed-chunk-27-1.png)<!-- -->

```
##      lhs    rhs                        support    confidence lift count
## [1]  {}  => {beverages}                0.02602949 0.02602949 1    256  
## [2]  {}  => {ice cream}                0.02501271 0.02501271 1    246  
## [3]  {}  => {specialty bar}            0.02735130 0.02735130 1    269  
## [4]  {}  => {misc. beverages}          0.02836807 0.02836807 1    279  
## [5]  {}  => {specialty chocolate}      0.03040163 0.03040163 1    299  
## [6]  {}  => {meat}                     0.02582613 0.02582613 1    254  
## [7]  {}  => {frozen meals}             0.02836807 0.02836807 1    279  
## [8]  {}  => {butter milk}              0.02796136 0.02796136 1    275  
## [9]  {}  => {candy}                    0.02989324 0.02989324 1    294  
## [10] {}  => {ham}                      0.02602949 0.02602949 1    256  
## [11] {}  => {UHT-milk}                 0.03345196 0.03345196 1    329  
## [12] {}  => {oil}                      0.02806304 0.02806304 1    276  
## [13] {}  => {onions}                   0.03101169 0.03101169 1    305  
## [14] {}  => {berries}                  0.03324860 0.03324860 1    327  
## [15] {}  => {hamburger meat}           0.03324860 0.03324860 1    327  
## [16] {}  => {hygiene articles}         0.03294357 0.03294357 1    324  
## [17] {}  => {salty snack}              0.03782410 0.03782410 1    372  
## [18] {}  => {sugar}                    0.03385867 0.03385867 1    333  
## [19] {}  => {waffles}                  0.03843416 0.03843416 1    378  
## [20] {}  => {long life bakery product} 0.03741739 0.03741739 1    368  
## [21] {}  => {dessert}                  0.03711235 0.03711235 1    365  
## [22] {}  => {canned beer}              0.07768175 0.07768175 1    764  
## [23] {}  => {cream cheese }            0.03965430 0.03965430 1    390  
## [24] {}  => {chicken}                  0.04290798 0.04290798 1    422  
## [25] {}  => {white bread}              0.04209456 0.04209456 1    414
```

We chose our parameters for the Apriori analysis by considering which combination would give us a large number of rules but wouldn't crash our computers. We were able to get over 50,000 rules and they gave us some interesting insights. One interesting finding was that many people go to the grocery store to buy one item. This makes sense because people often go to the store to get an item they forgot for a recipe, beer/beverages for parties, ice cream for lonely nights, snacks to cure the munchies, etc. We also found that there are 64 rules that we can be 100% confident about. For example, if someone buys rice and sugar, we can be confident that they'll buy whole milk. The bar graph earlier showed that whole milk is the most popular item bought by customers. That could explain why whole milk is so prevalent on this 100% confidence list. 

![](Figs/unnamed-chunk-28-1.png)<!-- -->

Here is a basic depiction of the network. It shows how many of the items are single purchases. It also shows that many of the traditional grocery shopping purchases (vegetables, buns, fruit, milk) are clustered together.



![](BasketTest4.png)

We made a more advanced nodes and edges graph and it shows us that whole milk is at the center. It is connected to many of the items in the graph and this makes sense based on our previous observations. We can also see some interesting clusters being formed. For example, cream cheese, whipped cream, and curd are close to long life bakery products. This means that a store could benefit from grouping these items together. Also, brown bread, frozen vegetables, and soft cheese are very close together in the graph. This means that these items are commonly bought together. We are confident that our analysis can help the store in this dataset decide where to put their items.
