**Collecting the total number of articles for each human gene (19504 human genes)**

**Load the following packages**

```
library(rvest)
library(dplyr)
library(readr)
```

**Read csv file**
```
humangenes <- read_csv("/Users/Desktop/human_proteins.csv")
View(humangenes)
colnames(humangenes)
```

**Select the genes and make them unique**
```
humangenes1 <- humangenes %>%
  dplyr::select(Locus)%>%
  unique()
allgenes <- humangenes1$Locus
pubmed_data <- data.frame()
```

#Start the process.
#This process depends on the speed of internet.

```
for(i in allgenes){
  link <- paste0("https://pubmed.ncbi.nlm.nih.gov/?term=", i,"&sort=date")
  page <- read_html(link)
  gene_name <- i 
  number <- page %>% html_nodes("span.value") %>% html_text()
  pubmed_data<- if(length(number) > 0)  {rbind(pubmed_data, data.frame(gene_name, number, stringsAsFactors = FALSE)) }
}
```

**Save the data**

```
write.csv(pubmed_data1, "/Users/Desktop/pubmed_data06.04.2022", row.names = FALSE)
```
**Create a plot **
**Load the following packages**

```
library(janitor)
library(cowplot)
library(tidyverse)
library(ggrepel)
```

**Generate the plot**

```
p <- ggplot(pubmed_genes, aes(x= fct_reorder(gene_name, number), y =number),
            alpha = 0.02 )+
  geom_point() +
  ylim(0, 30000)+
  geom_point(color = ifelse(pubmed_genes$number> 25000, "red", "black"))+
  theme_cowplot(font_size = 12)
```

**Label the X and Y axis**
```
p1 <- p + 
  theme(axis.text.x=element_blank(),
        axis.ticks.x = element_blank())+
  labs(x = "Human Genes", y = "Number of Articles", 
       title = "Number of Articles for each human gene")
```
**Mark gene names with different colot and label them**

```
p1 + geom_text_repel(aes(label=ifelse((pubmed_genes$number> 25000), gene_name, "")))

```

![Rplot01](https://user-images.githubusercontent.com/12661265/162208223-8ec1d69c-57e5-4f7d-9b52-117f928eccfb.png)