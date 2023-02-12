# FAPROTAX_Analysis
This code will use an ASV/OTU table as an input for FAPROTAX – a tool that estimates biological functions based on ASV/OTU tables.

```{FAPROTAX}

#Python using Windows PowerShell
##FAPROTAX
python collapse_table.py -i faprotax.taxon.table.tsv -o functional_table.tsv -g FAPROTAX.txt -c "#" -d taxonomy --omit_columns 0 --row_names_are_in taxonomy -r report.txt -n columns_after_collapsing -v -s out_subtables/ -f

otu.function.table <- read.table("functional_table.tsv", sep="\t", header=TRUE, row.names=1)

rel.otu.fn.df <- data.frame(otu.function.table)
rel.otu.fn.df$BioFunction = row.names(rel.otu.fn.df)
otu.melt <- melt(rel.otu.fn.df, id.vars = "BioFunction")
DFsub <- subset(otu.melt, value > 0)
write.csv(DFsub, "OTU_Melt_Trim2.Rare.csv")
COMP <- read.csv("OTU_Melt_Trim2.Rare.csv")

library(ggplot2)
#plot
ggplot(Comp[which(Comp$Count > 0),], aes(x=Compartment, y=BioFunction)) + geom_point(aes(colour= Compartment, size = Count)) + theme(axis.text.x = element_blank()) + ylab("") + theme_minimal() + xlab("") + scale_color_manual(values = wes_palette("Moonrise2", n=3)) + theme(axis.text.x = element_text(size = 10, face = "bold", angle = 45, vjust = 1, hjust = 1)) + theme(axis.text.y = element_text(size = 10, face = "bold")) + scale_y_discrete(labels = c("Aerobic Ammonia Oxidation", "Aerobic Chemoheterotrophy", "Aerobic Nitrite Oxidation", "Aliphatic Non-Methane Hydrocarbon Degradation", "Animal Parasites or Symbionts", "Anoxygenic Photoautotrophy", "Anoxygenic Photoautotrophy S Oxidizing", "Aromatic Compound Degradation", "Aromatic Hydrocarbon Degradtaion", "Cellulolysis", "Chemoheterotrophy", "Chitinolysis", "Dentrification", "Fermentation", "Human-Associated", "Human Pathogens", "Hydrocarbon Degradation", "Intracellular Parasites", "Iron Respiration", "Manganese Oxidation", "Methanol Oxidation", "Methanotrophy", "Methylotrophy", "Nitrate Dentrification", "Nitrate Reduction", "Nitrate Respiration","Nitrification", "Nitrite Dentrification", "Nitrite Respiration", "Nitrogen Fixation", "Nitrogen Respiration", "Nitrous Oxide Denitrification", "Non-Photosynthetic Cyanobacteria", "Photoautotrophy", "Photoheterotrophy", "Phototrophy", "Predatory or Exoparasitic", "Ureolysis", "Xylanolysis"))
