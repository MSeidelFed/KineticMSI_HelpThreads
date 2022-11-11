# KineticMSI usage of kReconstructMSI.R 
***Exemplary code to assist in reconstructing kMSI images either with SCilS generated coordinates or using Cardinal to extract them from the MSI files***

## Usage Instructions

Reconstructing image using SCiLS Coords (CSV files):

```{r}

test_reconstruct <- KineticMSI::kReconstructMSI(Reconstruct = "Before",
                                                kClustersMSI = 5,
                                                EnrPath = "OutputIsoCorrectoR/",
                                                GetCoords = "SCiLS",
                                                CoordsFilePath = "Coords_SCiLS/",
                                                PatternEnrichment = "MeanEnrichment.csv",
                                                FactorName = "test")


```

Reconstructing image using Cardinal Coords (MSI files):

```{r}

test_reconstruct <- KineticMSI::kReconstructMSI(Reconstruct = "Before",
                                                kClustersMSI = 5,
                                                EnrPath = "OutputIsoCorrectoR/",
                                                GetCoords = "Cardinal",
                                                CoordsFilePath = "Coords_Cardinal/",
                                                PatternEnrichment = "MeanEnrichment.csv",
                                                as = "MSImagingExperiment",
                                                FactorName = "test_Cardinal")


```

All the files necessary for these examples are self-contained in this repo upon downloading or cloning.
