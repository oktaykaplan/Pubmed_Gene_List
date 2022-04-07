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
