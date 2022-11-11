# KineticMSI workflow for k-Means segmentation of full images
***R code assisting [KineticMSI R package](https://github.com/MSeidelFed/KineticMSI)***

## Usage Instructions

Segmentation of the full enrichment matrix using k-Means from ComplexHeatmap:

```{r}

### Enrichment Files

Enrichment_files <- list.files(path = "OutputIsoCorrectoR/", pattern = "Enrichment.csv",
                               all.files = FALSE, full.names = TRUE, recursive = TRUE,
                               ignore.case = FALSE, include.dirs = TRUE, no.. = FALSE)

in_file_kmeans_Enr <- read.csv(file = Enrichment_files[1], header = T, row.names = 1)

### Corrected Fractions

Enrichment_files <- list.files(path = "OutputIsoCorrectoR/", pattern = "CorrectedFractions.csv",
                               all.files = FALSE, full.names = TRUE, recursive = TRUE,
                               ignore.case = FALSE, include.dirs = TRUE, no.. = FALSE)

## _0

in_file_kmeans_0 <- as.matrix(in_file_kmeans[grep("_0", x = rownames(in_file_kmeans)),])

in_file_kmeans_0[which(is.na(in_file_kmeans_0))] <- 0

## _1

in_file_kmeans_1 <- as.matrix(in_file_kmeans[grep("_1", x = rownames(in_file_kmeans)),])

in_file_kmeans_1[which(is.na(in_file_kmeans_1))] <- 0

## _1/_0

in_file_kmeans_0_1 <- as.matrix(in_file_kmeans[grep("_1", x = rownames(in_file_kmeans)),]/in_file_kmeans[grep("_0", x = rownames(in_file_kmeans)),])

in_file_kmeans_0_1[which(is.na(in_file_kmeans_0_1))] <- 0

#pdf("test_kMeans_m1.pdf")
#pdf("test_kMeans_m0.pdf")
#pdf("test_kMeans_m0_m1.pdf")
pdf("test_kMeans_m1_Enr.pdf")

for (i in 3:8) {

  ### run kmeans with the same nr of sig clusters from HCA bootstrap.

  #test_kmeans <- kmeans(x = t(in_file_kmeans_1), centers = i, iter.max = 100)
  #test_kmeans <- kmeans(x = t(in_file_kmeans_0), centers = i, iter.max = 100)
  #test_kmeans <- kmeans(x = t(in_file_kmeans_0_1), centers = i, iter.max = 100)
  test_kmeans <- kmeans(x = t(in_file_kmeans_Enr), centers = i, iter.max = 100)
  
  clusters_kmeans <- test_kmeans[["cluster"]]
  
  
  ### plots
  
  MSI_files <- list.files(path = "Coords_SCiLS/", pattern = ".csv",
                              all.files = FALSE, full.names = TRUE, recursive = TRUE,
                              ignore.case = FALSE, include.dirs = TRUE, no.. = FALSE)
  
  coords_file <- read.table(file = MSI_files[1], header = T, sep = ",") 
        
  x_coords <- ((trunc(coords_file$x) -
                        min(trunc(coords_file$x)))/10)/5
        
  y_coords <- ((trunc(coords_file$y) -
                        min(trunc(coords_file$y)))/10)/5
        
  df_coords_ <- rbind(x = x_coords,
                            y = y_coords)
        
  colnames(df_coords_) <- paste0("Spot.", coords_file$Spot.index)
        
        
  coords_ <- as.data.frame(cbind(x = df_coords_["x",], y = df_coords_["y",]),
                                 row.names = paste0("x =" , ", ",
                                                    df_coords_["x",],
                                                    "y =" , ", ",
                                                    df_coords_["y",],
                                                    "z =" , ", ",
                                                    rep(1, length(df_coords_["y",]))))
        
  df_coords = df_coords_
  
  
  Abscissas = c(min(as.numeric(df_coords["x",])) - 10,
                      max(as.numeric(df_coords["x",])) + 30)
  
  AbscissasContour = c(min(as.numeric(df_coords["x",])),
                       max(as.numeric(df_coords["x",])))
  
  Ordinates = c(min(as.numeric(df_coords["y",])),
                max(as.numeric(df_coords["y",])))
   
  plot_coords <- t(df_coords_)
   
  plot_colors <- clusters_kmeans
  
  ClustPlot <- plot(x = plot_coords[,"x"], y = plot_coords[,"y"],
                   col = plot_colors,
                   pch = 18,
                   #main = paste0("Cluster (#)", " - ", rownames(enrichmentFile)[i]),
                   xlim = Abscissas,
                   ylim = Ordinates,
                   xlab = "Abscissa coordinates", ylab = "Ordinate coordinates")
  
  legend("bottomleft", col = unique(plot_colors), pch = 18,
         legend = unique(clusters_kmeans))  
  
  
}

dev.off()

```

All files needed for the example are self-contained in this repo upon cloning or download.
