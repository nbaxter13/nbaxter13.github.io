---
layout: post
title: "Updated Models for CRC Diagnosis Using Microbiome Data"
author: "Niel"
date: "November 7, 2014"
output:
  html_document:
    keep_md: yes
---



I am trying to develop logit models for distinguishing healthy patients from those with colorectal cancer based on the abundances of bacterial populations in their gut microbiome.  To do this I would like to test all possible models containg 1-10 OTUs selected from the 309 most abundant OTUs.  Unfortunately that isn't feasible, due to the inordinantly large number of possible combinations.  

The number of combinations can be calculated using the formula n!/((n-r)!r!) where n is the number of OTUs to choose from (309) and r is the number of OTUs chosen for the model (1 to 10).  It can also be calculated with the choose() function in R.  I'll calculate how many models need to be tested for each number of OTUs (r=1 to r=10)


{% highlight r %}
for (r in 1:10) {
    cat(r, " OTUs: ", choose(309, r), "\n")
}
{% endhighlight %}



{% highlight text %}
## 1  OTUs:  309 
## 2  OTUs:  47586 
## 3  OTUs:  4869634 
## 4  OTUs:  372527001 
## 5  OTUs:  22724147061 
## 6  OTUs:  1.151357e+12 
## 7  OTUs:  4.98373e+13 
## 8  OTUs:  1.881358e+15 
## 9  OTUs:  6.292098e+16 
## 10  OTUs:  1.887629e+18
{% endhighlight %}

That is way too many models, so I need a way to run fewer of them while still finding the best (or nearly the best) models (i.e. a [heuristic](http://en.wikipedia.org/wiki/Heuristic_(computer_science))).

This is the heuristic I decided to use. First I calculated all possible 3-OTU models (4.869634 &times; 10<sup>6</sup> combinations). Then, rather than test all possible 4-OTU models (3.72527 &times; 10<sup>8</sup>), I took the top 100 of the 3-OTUs models and sequentially added each of the 309 OTUs to them.  This resulted approximately 3.09 &times; 10<sup>4</sup> 4-OTU models and saved lots of time.  I then took the 100 best of those 4-OTU models and again added each of the 309 OTUs to them.  I repeated this process up to 10-OTU models.

Here's an example for the 4 OTU models...


{% highlight r %}
library(gtools)
library(AICcmodavg)
{% endhighlight %}



{% highlight text %}
## Warning: no function found corresponding to methods exports from 'VGAM'
## for: 'show'
{% endhighlight %}



{% highlight r %}
setwd("~/Desktop/glne007/")
meta <- read.delim("training.meta.txt", header = T, sep = "\t")  #this file contains all of the metadata
shared <- read.delim("training.an.0.03.0.03.subsample.0.03.filter.shared", header = T, 
    sep = "\t")
shared <- shared[, -ncol(shared)]  #removes rareOtus column from the filtered shared file
shared <- shared[meta$dx != "adenoma", ]  #removed adenoma samples from shared file
meta <- meta[meta$dx != "adenoma", ]  #removed adenoma samples from metadata file

meta$dx <- as.character(meta$dx)
meta$dx[meta$dx == "normal"] <- 0
meta$dx[meta$dx == "cancer"] <- 1
meta$dx <- factor(meta$dx)
mydata <- data.frame(cbind(meta, shared[4:ncol(shared)]))
otus <- colnames(shared[4:ncol(shared)])
goodOTUs <- read.table("3otu.cancer.out")  #Reads in a list of all 3-OTU models sorted by AIC
goodOTUs <- goodOTUs[1:10, 1:3]  # For the sake of time i'm only taking the top 10 in this example

combos <- c()
for (x in otus) {
    # adds each of the 309 OTUs to the 100 top models
    com <- cbind(goodOTUs, x)
    combos <- rbind(combos, com)
}

getDuplicates <- function(x) {
    # finds models with duplicate OTUs
    return(length(x) - length(unique(unlist(x))))
}
dups <- apply(combos, MARGIN = 1, FUN = getDuplicates)
combos <- combos[!dups, ]  #removes models with duplicate OTUs

fun <- function(r) {
    # this function returns each of the OTUs that go into a model, and the AIC
    # for that model
    return(c(r[1], r[2], r[3], r[4], AICc(suppressWarnings(glm(dx ~ mydata[, 
        r[1]] + mydata[, r[2]] + mydata[, r[3]] + mydata[, r[4]], data = mydata, 
        family = binomial("logit"))))))
}  # the warnings I'm suppressing are 'glm.fit: fitted probabilities numerically 0 or 1 occurred'.  For our purposes, probabilities of 0 or 1 are great!
results <- apply(X = combos, MARGIN = 1, FUN = fun)
results <- t(results)
results <- results[order(results[, 5], decreasing = F), ]  #order results by AIC
colnames(results) <- c("OTU1", "OTU2", "OTU3", "OTU4", "AIC")
head(results, n = 10)
{% endhighlight %}



{% highlight text %}
##      OTU1        OTU2        OTU3        OTU4        AIC               
## 13   "Otu000035" "Otu000048" "Otu002051" "Otu000002" "196.118498930719"
## 1120 "Otu000002" "Otu000035" "Otu000048" "Otu002051" "196.118498930719"
## 553  "Otu000035" "Otu000048" "Otu002051" "Otu000063" "196.769064708944"
## 1114 "Otu000035" "Otu000048" "Otu000063" "Otu002051" "196.769064708944"
## 134  "Otu000035" "Otu000048" "Otu000063" "Otu000014" "197.14647774977" 
## 551  "Otu000014" "Otu000035" "Otu000048" "Otu000063" "197.14647774977" 
## 133  "Otu000035" "Otu000048" "Otu002051" "Otu000014" "198.783998823214"
## 1111 "Otu000014" "Otu000035" "Otu000048" "Otu002051" "198.783998823214"
## 83   "Otu000035" "Otu000048" "Otu002051" "Otu000009" "198.892468636466"
## 2174 "Otu000035" "Otu000048" "Otu000063" "Otu002974" "198.954604800836"
{% endhighlight %}



###ROC Curves for models with 3, 5, 7, or 10 OTUs
Using the best models I found with 3-10 OTUs (though not necessarily the absolute best models, since I didn't test all possible combinations of OTUs), I generated Reciever Operator Characteristic (ROC) curves.  I'll only show the code for the first plot.



{% highlight r %}
library(pROC)
setwd("~/Desktop/glne007/")
meta <- read.delim("training.meta.txt", header = T, sep = "\t")
shared <- read.delim("training.an.0.03.0.03.subsample.0.03.filter.shared", header = T, 
    sep = "\t")
shared <- shared[meta$dx != "adenoma", ]
meta <- meta[meta$dx != "adenoma", ]

meta$dx <- as.character(meta$dx)
meta$dx[meta$dx == "normal"] <- 0
meta$dx[meta$dx == "cancer"] <- 1
meta$dx <- factor(meta$dx)
mydata <- data.frame(cbind(meta, shared[4:ncol(shared)]))

logit1 <- glm(dx ~ Otu000014 + Otu000035 + Otu000048, data = mydata, family = binomial("logit"))
logit2 <- glm(dx ~ Otu000035 + Otu000048 + Otu002051 + Otu000063 + Otu000014, 
    data = mydata, family = binomial("logit"))
logit3 <- glm(dx ~ Otu000035 + Otu000048 + Otu002051 + Otu000063 + Otu002955 + 
    Otu000057 + Otu000014, data = mydata, family = binomial("logit"))
logit4 <- glm(dx ~ Otu000035 + Otu000048 + Otu002051 + Otu000063 + Otu002955 + 
    Otu000057 + Otu000053 + Otu004344 + Otu000176 + Otu003036, data = mydata, 
    family = binomial("logit"))

prob1 = predict(logit1, type = c("response"))
prob2 = predict(logit2, type = c("response"))
prob3 = predict(logit3, type = c("response"))
prob4 = predict(logit4, type = c("response"))
mydata$prob1 = prob1
mydata$prob2 = prob2
mydata$prob3 = prob3
mydata$prob4 = prob4
roc1 <- roc(dx ~ prob1, data = mydata)
roc2 <- roc(dx ~ prob2, data = mydata)
roc3 <- roc(dx ~ prob3, data = mydata)
roc4 <- roc(dx ~ prob4, data = mydata)

par(mar = c(5, 5, 4, 1))
plot(0, type = "n", xlim = c(1.01, 0), ylim = c(-0.01, 1.01), xaxs = "i", yaxs = "i", 
    ylab = "Sensitivity", xlab = "Specificity", main = "Cancer vs. Healthy: Microbiome Only")
plot(roc1, col = "orange", lwd = 2, add = T)
plot(roc2, col = "red", lwd = 2, add = T)
plot(roc3, col = "blue", lwd = 2, add = T)
plot(roc4, col = "dark green", lwd = 2, add = T)
points(abline(1, -1), col = "grey")
legend("bottomright", col = c("orange", "red", "blue", "dark green"), lty = c(1, 
    1, 1, 1), lwd = 3, legend = c(sprintf("3 OTUs: AUC = %.3g", roc1$auc), sprintf("5 OTUs: AUC = %.3g", 
    roc2$auc), sprintf("7 OTUs: AUC = %.3g", roc3$auc), sprintf("10 OTUs: AUC = %.3g", 
    roc4$auc)))
{% endhighlight %}

<img src="/../figs/cancerHealthy-1.png" title="center" alt="center" style="display: block; margin: auto;" />


<img src="/../figs/adenomaHealthy-1.png" title="center" alt="center" style="display: block; margin: auto;" />


<img src="/../figs/lesionHealthy-1.png" title="center" alt="center" style="display: block; margin: auto;" />


I will probably go back and rerun the models with the same heuristic, but take the top 1000 models from each step rather than the top 100.  That will require running the scripts on axiom, which I haven't tried yet.  Although the current models may not be absolute best, they are significantly better than the models I showed in my 812. The next step will be to add FIT results and demographic information to the models.


