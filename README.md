# Identify-genomic-features-by-CNN
This is just an experimental approach to convert DNA sequences into images or arrays. Generated data was used to train a neural network for further feature extraction.

Berlin,, 2023
Dr. Ali Rahjouei

## Introduction
Deoxyribonucleic acid (DNA) is a polymer composed of adenine (A) or thymine (T), cytosine (C) and guanine (G), a sugar called deoxyribose, and a phosphate group. Repeat of these unites through the entire genome generate codes essential for all cellular activities. This information can be read and translated into proteins as molecules with many different roles and functions. Just a small portion of genome consist of coding sequences and the rest of genome has structural and regulatory roles. Genomic duplications, deletions, rearrangements and mutations are the main source of genomic diversification during species evolution. The footprint of events can be observed as repetitive patterns through the entire genome. Under evolutionary pressure, some of these patterns or repeats remain intact which, can be recognized by different DNA-binding proteins and have a fundamental role in gene regulation and chromatin biology. Identifying DNA features is a difficult and resource consumable tasks.

Our recent advancement in unsupervised image classification methods inspired me to use these methods to identify genomic features by converting genetic information to arrays or figures. Then use the generated dataset to train a neural network and extract recognized genomic features and classify the segmented genomic by k-mean clustering. 

![alt text](https://db3pap006files.storage.live.com/y4m3maGE76ySwNzy9fb3_UsoEq2H5UCNwDLYmiAo_nSZK0sFMJcTeAioVNaZeCVR92HYEqG9ZSRZ3b40zWh-w1py-51R_V9m8LlfU8GOBy5wWLSw5rZqfRhkkt8-tuD7qYnwOmZu9qhPR3f_RHL7vhzFOi3BmL5gZkyOIIZ335vsqjYkUsXl1IkUQFkg925p2sd?width=1522&height=507&cropmode=none)

## Converting DNA sequences to image in R
1) The idea is to read the reference genome in fasta format (for example:https://ftp.ensembl.org/pub/release-108/fasta/drosophila_melanogaster/) and convert a, t, c and g to a vector or list containing integers (1, 2, 3, 4).
2) Converting vector or list to array: array dimentions
3) Converting array to .png images.

Codes in R:
```
#loading library
library(seqinr)

#loading DNA sequence in fasta format
DF = read.fasta(file = "/Users/arahjou/Downloads/Drosophila_melanogaster.BDGP6.32.dna.toplevel.fa.gz")
DF = read.fasta(file = "/Users/arahjou/Library/CloudStorage/OneDrive-SharedLibraries-Onedrive/2023\ Projects/Genomic\ Image\ classification/Drosophila_melanogaster.BDGP6.32.dna.toplevel.fa.gz")

# Take a look at the number of base pairs
length(DF$`2L`)
#Check your files
DF$`2L`[1:50]


#==================== creating array and image  =======================
#This block read number of bases in each chromosome and 
# generate images

a=1#initial number
b=10000#initial number

#Defining length of vector and number of loop
Final.number = length(DF$`2L`)
loop.number = round(length(DF$`2L`)/10000)

if (b > Final.number) {
  Print("Done!")
} else {
  for (i in 1:loop.number){
    x= ifelse(DF$`2L`[a:b] == "a", 1,
              ifelse(DF$`2L`[a:b] == "t", 2,
                     ifelse(DF$`2L`[a:b] == "c", 3, 4)))
    a=a+10000
    b=b+10000
    
    #Converting vector to a matrix
    x <-matrix(x,nrow=100, byrow=TRUE)
    
    #Make a new path to save image
    PATH = paste0("/Users/arahjou/Downloads/Files/", "bin",a,".png")
    png(file = PATH)
    image(x,col=grey.colors(4), axes=FALSE)
    dev.size()
    dev.off()
  }
}
```
## Image Pre-processing
- Bluring:
- Edge detection:

## Projects:
#### Discovering features of DNA sequences: 
- Fixed frame: 
- Shifting frame:
#### Identifying DNA from different species:
#### Adding bigwig information on top of DNA sequence frame:
