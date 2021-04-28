I am trying to learn about different ways to visualize gene expression data and saw some papers using chord plots. These plots can help show relationships between genes and the gene ontogeny terms they are associtaed with. 

This is useful beacause the results of differential gene expression analysis can be overwhelming (many genes may be up/down regulated) and the different gene names an ontogeny terms are not familiar (at least to me). 

Below is an image of a chord plot I liked from the PNAS paper 'RNA-seq reveals conservation of function among the yolk sacs of human, mouse, and chicken' by Tereza Cindrova-Davies, Eric Jauniaux, Michael G. Elliot, Sungsam Gong, Graham J. Burton, and Stephen Charnock-Jones. This study used transcriptomic data from human, mouse, and chicken yolk sacs to reveal genetic similarities across mammals and birds (which supports their hypothesis of conservation of function).  The authors looked at the top 400 enriched transcripts and their associated GO terms, and then visualized all genes whose GO terms included the word cholestrol. Cholestrol is essential for yolk sacs and developing empbryos, so this choice makes sense.

![chordplot](/Images/ChordPlot.png)

I tried making my own chord plot with bird transcriptomic data, but the process showed me that I need to give myself more time to think through with biological processes, cellular components, and molecular functions are relevant for the questions in my study. Visualizing RNAseq data seems like the most customizable data visualization I've ever experienced and I think that makes it much much harder. The high-dimensionality of the data makes me really uncomfortable actually! It seems impossible to visualize ALL the data (at least with functional information included) and being forced to choose what to display is a challenge (how to choose the right things?). I haven't looked into this yet but a shiny app for chord visualizations would be neat. 

I have struggled on my way to my own chord plot, so I'm not going to paste in my embarrassingly crappy plot here. But when I sort it out, I hope to be able to show up to 5 selected GO terms, and the genes they are related to, as well as a circular heatmap to indicate how those genes/GOterms are up or down-regulated in my contrast (parasitized birds that live in an urban area versus parasitized birds that live in a more natural area). 

I've been using the R package 'GOPlot' to try to make my chord plots. My main issue so far is that I need to narrow down the genes I want to look at (this will take me a while), and I am having a coding error (I'm using a slighlty different data structure than what's used in the package vignette) and haven't yet figured out a fix. 