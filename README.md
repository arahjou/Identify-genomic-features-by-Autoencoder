# Identify-genomic-features-by-CNN
This is just an experimental approach to convert DNA sequences into images or arrays. Generated data was used to train a neural network for further feature extraction.

Berlin,, 2023
Dr. Ali Rahjouei

## Introduction
Deoxyribonucleic acid (DNA) is a polymer composed of adenine (A) or thymine (T), cytosine (C) and guanine (G), a sugar called deoxyribose, and a phosphate group. Repeat of these unites through the entire genome generate codes essential for all cellular activities. This information can be read and translated into proteins as molecules with many different roles and functions. Just a small portion of genome consist of coding sequences and the rest of genome has structural and regulatory roles. Genomic duplications, deletions, rearrangements and mutations are the main source of genomic diversification during species evolution. The footprint of events can be observed as repetitive patterns through the entire genome. Under evolutionary pressure, some of these patterns or repeats remain intact which, can be recognized by different DNA-binding proteins and have a fundamental role in gene regulation and chromatin biology. Identifying DNA features is a difficult and resource consumable tasks.

Our recent advancement in unsupervised image classification methods inspired me to use these methods to identify genomic features by converting genetic information to arrays or figures. Then use the generated dataset to train a neural network and extract recognized genomic features and classify the segmented genomic by k-mean clustering. 

## Converting DNA sequences to array
The idea is to read refernce geneome in fasta format ( for example:https://ftp.ensembl.org/pub/release-108/fasta/drosophila_melanogaster/) and convert a, t, c and g to a vector or list containing integers (1, 2, 3, 4).
## Convering vector or list to array
To generat enough data to 
