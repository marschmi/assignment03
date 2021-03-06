# Assignment 3
Marian L Schmidt  

####1.  Generate a plot that contains the different pch symbols. Investigate the knitr code chunk options to see whether you can have a pdf version of the image produced so you can print it off for yoru reference. It should look like this:

    <img src="pch.png", style="margin:0px auto;display:block" width="500">



```r
symbol <- c(1:25)
y <- rep(1, 25)
plot(symbol, y, type = "n", xlab = "PCH value", xlim=c(0,25), frame = F, yaxt = "n", xaxt="n", ylab="", main="PCH Symbols")
axis(side=1, at=1:25)# abs=c(seq(1, 25, 2))side = 1, 1:25
     #lab=c("1", "", "3", "", "5", "", "7", "", "9", "", "11", "", "13", "", "15", "", "17", "", "19", "", "21", "", "23", "", "25")) 
abline(v=seq(1:25), b=0, col = "gray")
points(symbol,y, pch = c(1:25), cex=2)
```

<img src="README_files/figure-html/unnamed-chunk-1-1.png" title="" alt="" style="display: block; margin: auto;" />

For frame = F:  http://stackoverflow.com/questions/4946491/removing-the-frame-from-the-boxplot-function-in-r
Axes: http://www.statmethods.net/advgraphs/axes.html




####2.  Using the `germfree.nmds.axes` data file available in this respositry, generate a plot that looks like this. The points are connected in the order they were sampled with the circle representing the beginning ad the square the end of the time course:

    <img src="beta.png", style="margin:0px auto;display:block" width="700">



```r
germfree <- read.table(file = "germfree.nmds.axes", header = T)

#First, let's subset the data by mouse
mouse337 <- subset(germfree, mouse == "337")
mouse343 <- subset(germfree, mouse == "343")
mouse361 <- subset(germfree, mouse == "361")
mouse387 <- subset(germfree, mouse == "387")
mouse389 <- subset(germfree, mouse == "389")

par(font=2, family = "Helvetica") # bold legend
plot(germfree$axis1, germfree$axis2, type = "n", xlab = "NMDS Axis 1", ylab = "NMDS Axis 2", font=2, font.lab=2) #bolded axes

#Draw lines of the timecourse for each mouse through NMDS space
points(mouse337$axis1, mouse337$axis2, type="l", col="black", lwd=2)
points(mouse343$axis1, mouse343$axis2, type="l", col="blue", lwd=2)
points(mouse361$axis1, mouse361$axis2, type="l", col="red", lwd=2)
points(mouse387$axis1, mouse387$axis2, type="l", col="green", lwd=2)
points(mouse389$axis1, mouse389$axis2, type="l", col="darkred", lwd=2)

#Time to add the circles to represent the beginning of the time course
points(mouse337$axis1[1], mouse337$axis2[1], type = "p", col = "black", pch=19, cex=2)
points(mouse361$axis1[1], mouse361$axis2[1], type = "p", col = "red", pch=19, cex=2)
points(mouse343$axis1[1], mouse343$axis2[1], type = "p", col = "blue", pch=19, cex=2)
points(mouse387$axis1[1], mouse387$axis2[1], type = "p", col = "green", pch=19, cex=2)
points(mouse389$axis1[1], mouse389$axis2[1], type = "p", col = "darkred", pch=19, cex=2)

#Add squares to represent the end of the time course
points(mouse337$axis1[20], mouse337$axis2[20], type = "p", col = "black", pch=15, cex=2)
points(mouse343$axis1[21], mouse343$axis2[21], type = "p", col = "blue", pch=15, cex=2)
points(mouse361$axis1[20], mouse361$axis2[20], type = "p", col = "red", pch=15, cex=2)
points(mouse387$axis1[21], mouse387$axis2[21], type = "p", col = "green", pch=15, cex=2)
points(mouse389$axis1[21], mouse389$axis2[21], type = "p", col = "darkred", pch=15, cex=2)

#Finally, let's add the legend
legend(x=-0.02, y=-0.18, 
       legend=c("Mouse 337", "Mouse 343", "Mouse 361", "Mouse 387", "Mouse 389"), 
       col=c("black", "blue", "red", "green", "darkred"), lty=1, lwd=4)
```

<img src="README_files/figure-html/unnamed-chunk-2-1.png" title="" alt="" style="display: block; margin: auto;" />



####3.  On pg. 57 there is a formula for the probability of making x observations after n trials when there is a probability p of the observation.  For this exercise, assume x=2, n=10, and p=0.5.  Using R, calculate the probability of x using this formula and the appropriate built in function. Compare it to the results we obtained in class when discussing the sex ratios of mice.



```r
n_trials <- 10
prob <- 0.5
x_observ <- 2
woot <- dbinom(x = x_observ, size = n_trials, prob = prob)
percent <- woot*100
```

**Answer:** Here, we performed 10 breedings of mice but only observed 2 of the breedings.  The probability of getting a male mouse versus a female mouse was 50/50.  This calculation was the same as what we did in class, with a point probability of 0.0439453.  This means that 4.3945312% of the time we will get either 2 females or 2 males from our 2 observations.


####4.  On pg. 59 there is a formula for the probability of observing a value, x, when there is a mean, mu, and standard deviation, sigma.  For this exercise, assume x=10.3, mu=5, and sigma=3.  Using R, calculate the probability of x using this formula and the appropriate built in function



```r
x_obs <- 10.3
st_dev <- 3
mean <- 5
test <- dnorm(x_obs, mean, st_dev)
```

**Answer:** The probability is 0.0279285.

####5.  One of my previous students, Joe Zackular, obtained stool samples from 89 people that underwent colonoscopies.  30 of these individuals had no signs of disease, 30 had non-cancerous ademonas, and 29 had cancer.  It was previously suggested that the bacterium *Fusobacterium nucleatum* was associated with cancer.  In these three pools of subjects, Joe determined that 4, 1, and 14 individuals harbored *F. nucleatum*, respectively. Create a matrix table to represent the number of individuals with and without _F. nucleatum_ as a function of disease state.  Then do the following:


```r
#Make a Matrix with data
Group <-  c("NoDisease", "Ademonas", "Cancer")
Num_People <- c(30, 30, 29)
Nucleatum_Abund <- c(4, 1, 14)
nuc <- data.frame(Group, Num_People, Nucleatum_Abund)
rownames(nuc) <- nuc$Group
nuc$Group = NULL
data.matrix(nuc, rownames.force = T)
```

```
##           Num_People Nucleatum_Abund
## NoDisease         30               4
## Ademonas          30               1
## Cancer            29              14
```

####5a. Run the three tests of proportions you learned about in class using built in R functions to the 2x2 study design where normals and adenomas are pooled and compared to carcinomas.

**1.  Test of proportions**:  Give two vectors where the first contains the number of positive outcomes and the second the total number for each group.   

+ **Null Hypothesis:** The proportion of healthy individuals with _F. nucleatum_ is the same as the proportion of cancerous individuals with *F. nucleatum.*   
+ **Alternative Hypothesis:**  The proportion of healthy individuals with _F. nucleatum_ is **not the same** as the proportion of cancerous individuals with *F. nucleatum.*   

```r
#Test of proportions
with_nuc <- c(5,14) #Successes
total <- c(60,29) #Totals
nuc_proptest <- prop.test(with_nuc, total)
```
**Answer with test of proportions:** With a test of proportions, we get a p-value of 5.4822381\times 10^{-5}, which tells us that our null hypothesis is rejected.  Thus, there is a significant difference between the proportion of healthy individuals with _F. nucleatum_ and the proportion of cancerous individuals with _F. nucleatum_.

***  

**2.  Fisher's Exact Test**:  Requires data to be in matrix form (2X2) where the first column is the number of successes and the second column is the number of failures. (Success = With _F. nucleatum_, Failure = Without _F. nucleatum_).  

+ **Null Hypothesis:** Cancer **does not** affect the presence of _F. nucleatum_ in people.   
+ **Alternative Hypothesis:**  Cancer **does** affect the presence of _F. nucleatum_ in people.  


```r
#Fisher's Exact Test
nuc <- matrix(c(5, 14, 55,15), 2)
nuc_fishertest <- fisher.test(nuc)
```
**Answer With Fisher's exact test:**  A p-value of 4.0941172\times 10^{-5} tells us that our null hypothesis is rejected.  So, cancer does affect the presence of _F. nucleatum_ in people. 

***

**3.  Binomial Test**:  Requires data to be in matrix form (2X2) where the first column is the number of successes and the second column is the number of failures. (Success = With _F. nucleatum_, Failure = Without _F. nucleatum_)  

+ **Null Hypothesis:** The probability of  _F. nucleatum_ presence in healthy people/people with cancer is **less than 0.5.**      
+ **Alternative Hypothesis:**  The probability of _F. nucleatum_ presence in healthy people/people with cancer is **more than 0.5.**    


```r
#Binomial test
cancer_binom <- binom.test(14, 15, 0.5, alternative ="greater")
healthy_binom <- binom.test(5, 55, 0.5, alternative = "greater")
```
**Answer with a binomial test:**  For individuals with cancer, a p-value of 4.8828125\times 10^{-4} tells us that our null hypothesis is rejected.  So, The probability of _F. nucleatum_ presence in people with cancer is more than 0.5.  For healthy individuals, we get a p-value of 1, which accepts our null hypothesis.  So, the probability of _F. nucleatum_ presence in people with cancer is less than 0.5. 


####5b. Without using the built in chi-squared test function, replicate the 2x2 study design in the last problem for the Chi-Squared Test...  Calculate the expected count matrix and calculate the Chi-Squared test statistics. Figure out how to get your test statistic to match Rs default statistic.  

**Chi-squared test:** Test if distributions of categorical variables differ from one another.  Needs count data, not proportions.   

+ **Null Hypothesis:** There **is no** association between cancer and _F. nucleatum_ presence.   
+ **Alternative Hypothesis:**  There **is** an association between cancer and _F. nucleatum_ presence.    

```r
#Manual Chi-squared Test
mat <- matrix(c(5, 14, 19, 55,15, 70, 60, 29, 89), 3)
a<-5; b<-55; c<-14; d<-15; n <- 89; 
Ns <- 19; Nf <- 70; Na <- 60; Nb <- 29
sq <- ((a*d-b*c)^2)
Xsq <- (((sq)*(a+b+c+d))/((a+b)*(c+d)*(a+c)*(b+d)))

#Expected frequencies:  E(r,c) = (N(r)*n(c))/n 
Ea <- (60*19)/n
Eb <- (60*70)/n
Ec <- (29*19)/n
Ed <- (29*70)/n
blarg <- (((a^2-Ea)/Ea)+((b^2-Eb)/Eb)+((c^2-Ec)/Ec)+((d^2-Ed)/Ed))

#Calculate the degrees of freedom: df = (ncol-1)*(nrow-1) -> no total columns/rows included
df <- (2-1)*(2-1)

#R's Default Chi-squared Test
nuc <- matrix(c(5, 14, 55,15), 2)
nuc_chisqtest <- chisq.test(nuc)

#Yate's Continuity Correction of Manual Chi-Squared Test
numerator <- n * (abs(a*d-b*c) - (n/2))^2
denominator <- Ns*Nf*Na*Nb
yates <- numerator/denominator
```
  _Help from [link](http://math.hws.edu/javamath/ryan/ChiSquare.html) for manual chi-squared test._
  _Help from [link](http://en.wikipedia.org/wiki/Yates%27s_correction_for_continuity) for Yate's continuity correction._  


**Answer:** The two answers do not match because of Yates continuity correction.  With the corrected X-squared value, 16.2736031 equals the default statistic.  

+ **Manual chi-squared test:**  X-square value of 18.5762791 with 1 degree of freedom.  
+ **R's default chi-squared statistic:**  X-square value of 16.2736 with 1 degree of freedom.  
  
####5c. Generate a Chi-Squared distributions with approporiate degrees of freedom by the method that was discussed in class (hint: you may consider using the `replicate` command)

```r
#Above we calculated that df = 1
chisq_dist <- replicate(1000, sum((rnorm(1,0,1))^2))
chisq_hist <- hist(chisq_dist,breaks=1000, xlim = c(0, 20), main = "My Chi-Squared Distribution",
     xlab = "Chi-Squared Statistic"); box()
```

![](README_files/figure-html/unnamed-chunk-11-1.png) 

####5d. Compare your Chi-Squared distributions to what you might get from the appropriate built in R functions

```r
x_values <- seq(0, 20, 0.05)
default_chisq_dist <- dchisq(seq(0, 20, 0.05), df = df)
plot(x_values, default_chisq_dist, type = "l", xlab = "Chi-Squared Statistic", ylab = "Probability (df = 1)",  main = "R's Default Chi-Squared Distribution")
arrows(x0 = yates, x1 = yates, y0 = 0.4, y1 = 0.05, lwd = 2, col = "blue")
arrows(x0 = Xsq, x1 = Xsq, y0 = 0.5, y1 = 0.05, lwd = 2, col = "red")
text(x = yates, y= 0.45, labels = "Yates", col = "blue")
text(x = Xsq, y= 0.55, labels = "No Yates", col = "red")
```

![](README_files/figure-html/unnamed-chunk-12-1.png) 

**Answers to 5c and 5d:**  While my chi-squared distribution appears to be similar to the default R distribution, there are a few differences.  I have a discrete frequency distribution while the default is a continuous probability distribution.  


####5e. Based on your distribution calculate p-values

```r
#First make a table of the density distribution of our manually calculated chi-sq
density_table <- cbind(x=seq(from=0, to=20, by=20/(length(chisq_hist$density)-1)), y=chisq_hist$density)
density_table <- data.frame(density_table) #make table subset-able
p_density <- subset(density_table, density_table$x >Xsq) #take all values that are greater than our manually calculated chi-squared statistic
manual_p_value <- sum(p_density$y)/sum(density_table)
```

####5f. How does your p-value compare to what you saw using the built in functions? Explain your observations.
**Answer:**  The default R-calculated chi-square test p-value is 5.4822381\times 10^{-5}  while my calculated p-value is 7.5018755\times 10^{-6}.  My calculated p-value is lower than the R default p-value.  


####6.  Get a bag of Skittles or M&Ms.  Are the candies evenly distributed amongst the different colors?  Justify your conclusion.

Here, I will use a test of proportions to see if the distribution of M&M's in the bag is even.

+ **Null Hypothesis:** M&M colors in the bag are evenly distributed.  
+ **Alternative Hypothesis:** M&M colors in the bag are **not** evenly distributed.


```r
#Create a table with the amount of mms for each color
mms <- matrix(c(9, 13, 6, 11, 8, 10), ncol = 1)
mms <- data.frame(mms)
rownames(mms) <- c("Green", "Orange", "Red", "Blue", "Yellow", "Brown")

#If they were evenly distributed
even_total <- sum(mms)
color_eventotal <- even_total/length(rownames(mms))
#Let's add a row to the table with this total
mms <- cbind(mms, color_eventotal)
colnames(mms) <- c("Bag", "Even")

#Let's get a visual of our 2 distributions
barplot(t(mms), main="M&M's Color Distribution", ylim = c(0, 15), ylab = "Number of M&M's",
  xlab="Color of M&M's",col = c("blue4", "violetred"), beside=TRUE, legend = FALSE); box()
legend("topright",title = "Distribution", c("Bag","Even"), fill=c("blue4", "violetred"), horiz=TRUE)
```

![](README_files/figure-html/unnamed-chunk-14-1.png) 

```r
#Get the proportions of each color of mm
bag <- mms$Bag
even <- mms$Even
prop_bag <- prop.table(bag) #get proportion of each color.
prop_even <- prop.table(even)

#Run the test of proportions:
totals <- rep(sum(mms$Bag), length(mms$Bag))
mm_test <- prop.test(bag, totals, prop_even)
```

_Help from [link](http://www.ats.ucla.edu/stat/r/faq/barplotplus.htm) for producing figure._


**Answer:**  Through a test of proportions with an insignificant p-value of 0.7136558, I cannot reject that colors are evenly distributed.

