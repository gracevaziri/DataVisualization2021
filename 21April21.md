---
title: "HW 20 April"
author: "Grace Vaziri"
date: "4/20/2021"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
*This post is more of a journal entry of a process I recently went through, that would have been impossible without using quick data-visualization techniques for data exploration.*

I have been analyzing gene expression data for some birds. I'm interested in two treatments (parasitized and not-parasitized) two sites (urban and non-urban), and two age groups . Using DESEq2, I'm analyzing differential expression of genes based on count data from sequencing. Doing some pairwise comparisons makes most sense for these data, so I've been subsetting them based on the particular comparisons I care about for each analysis. I was most excited to compare parasitized nestlings between the two sites so I started there. One of my first steps was to make a PCA to see if there was any interesting or unexpected clustering of samples.

Using all nestlings and the following code, I found this pattern.

![plot1](/Images/21AprImg1.png)
```{r}
dat_W <- plotPCA(vsd_Water, intgroup="site", returnData=TRUE)

p <- ggplot(dat_W,aes(x=PC1,y=PC2,color = site))
(p <- p + geom_point(size = 3)+
    theme_minimal()+
    theme(plot.background = element_blank()))
```
So it looks like there is definitely a separation of the samples, but it is not based on the site a nestling came from, or treatment (because I'm only visualizing parasitized nestlings here).

I wanted to see if this separation was something that showed up in other sample types as well (when I saw this I was quite worried about a potential batch effect at the RNA extraction step). I made a new plot with all my samples (adult, nestling, parasitized, not-parasitized, urban, non-urban). 

```{r}
#plot all birds
dat_all <- plotPCA(vsd_all, intgroup="condition", returnData=TRUE)

p3 <- ggplot(dat_all,aes(x=PC1,y=PC2,color = condition))
(p3 <- p3 + geom_point(size = 3)+
    theme_minimal()+
    theme(plot.background = element_blank()))
```
![plot2](/Images/21AprImg2.png)
This plot had me really worried because I couldn't see any clustering by site or condition! Neither were points clustering by age. 

I decided to extract the axis loadings for the PC2 axis so I could investigate whether the samples seemed to have any unintended grouping variables. I planned to look at all samples whose PC2 loading was >0 to see whether they were different than the samples with a PC2 loading of <0. 

I checked when RNA samples were extracted, which box samples had been stored in for freezer storage, the date samples were collected, the person by whom samples were collected, and oh, yeah, the sex of the bird the sample came from.

Only adult birds had sex listed, but I finally tried looking at whether sex was grouping samples together. And what do you know... 

```{r}
dat_adult <- plotPCA(vsd_adult, intgroup="sex", returnData=TRUE)

adult <- ggplot(dat_adult,aes(x=PC1,y=PC2,color = sex))
(adult <- adult + geom_point(size = 3)+
    theme_minimal()+
    theme(plot.background = element_blank()))

#which female is clustering with the males?
dat_adult[dat_adult$PC2>10,] #SAK0165... let's look at the notes for this bird
#notes say "looks a bit like [a sympatric and very similar species]"...interesting

```
![plot3](/Images/21AprImg3.png)
This graph of all adult bird samples shows that perhaps sex is important factor controlling gene expression. That should not be surprising, but I think it was a good lesson to me to keep biology basics top of mind, even when learning cool new analyses. 

Finally-now I'm wondering if I mis-sexed a bird in the hand while doing field work! What's that pink dot doing up in the blue cloud? Back to my field notes to check what I wrote down for that bird. Apparently I wrote that the bird looked a little bit like another species that is sympatric with my focal species. If so, I may have mis-sexed it, or mis-identified it. I will have to look into this further.



