I am doing my best-practice commentary and my figure code on heatmap figures for differential gene expression this week. I read a paper by Hannah Watson, Elin Videvall, Martin Andersson, and Caroline Isaksson about detecting differentially expressed genes in a population of urban Great Tits (*Parus major*).

The authors used blood and liver to look at gene transcription profiles and found differences in gene expression based on where the birds lived. Urban birds showed enhanced expression of genes associated with stress. The authors had one of the prettiest heat maps I have seen, and I wanted to try to recreate it for some work I'm doing on urban and rural birds.

Here's their graph. Panel A shows genes from liver, and B from blood. The authors show all the differentially expressed genes from each tissue in the heatmaps, so there is not space for gene names. This makes sense because heatmaps generally serve to help us see broad patterns.

![WatsonHeatmap](Images/watsonheatmap.png)

My version was slightly different-but very inspired by the above plot. I chose to just show the top 25 DGEs, and to show their names. I also included a split to indicate the first where hierarchical clustering would separate my samples into two groups, based on similarities in gene expression. I included a scale to show which site a sample came from. I didn't include sample ID, because these data aren't yet published. The graph below helped me see that one sample from my OG site is much different from the others, and that it is more similar to samples from the PB site. I chose not to include all the DGEs because I like being able to see the gene names. I think when I explore GO terms and pathways later on, it will be cool to be able to back to my heatmap to see these data.

I included the code I used to create the figure at the end of this post-but the data are not ready to be shared.

![myheatmap](Images/top25DEGs_heatmap.png)


```{r}# heatmap of DE genes
# I used a DESeqDataSet data object to make this graph
# the data object included a matrix of genes and samples, filled in with counts/reads
# It also includes associated gene names, statistics about differential expression for 
# each gene, and metadata associated with samples (treatment/site/etc.)


# I transformed the counts with a regularized log transformation 
rld_W <- rlog(dds_nestling_Water, blind=FALSE)

# get top 50/25/10 genes based on log2 fold change
top50_W <- order(-abs(res_shrink_W$log2FoldChange))[1:50]
top25_W <- order(-abs(res_shrink_W$log2FoldChange))[1:25]
top10_W <- order(-abs(res_shrink_W$log2FoldChange))[1:10]

# get a dataframe with the sample metadata variable I care about visualizing (in this case, site)
df <- data.frame(colData(dds_nestling_Water)[,"site"])
rownames(df) <- colnames(dds_nestling_Water)
colnames(df) <- "site"

#customize the color option for building my heatmap
#specify colors
RColorBrewer::display.brewer.all()
(col.pal <- RColorBrewer::brewer.pal(9, "Greens"))

# make the heatmap
pheatmap(
assay(rld_W)[top25_W,], #showing the top 25 genes felt most informative without being too much on the eyes
  cluster_rows=FALSE, #I'm not showing any functional data for genes so no point in clustering here
  show_rownames=TRUE, #interesting to see the names though
  show_colnames = FALSE, # not showing sample names bc this data isn't yet published
  cluster_cols=TRUE, #I did want to show how the samples are similar/dissimlar based on DGE
  annotation_col = df, #Include a color scale to indicate site
cutree_cols = 2, #include a visual break between the two highest-level clusters in samples
color = col.pal #indicates the color pallet I chose to use
)
```
