# Identify-genomic-features-by-CNN
This is just an experimental approach to convert DNA sequences into images or arrays. Generated data was used to train a neural network for further feature extraction.

Berlin,, 2023
Dr. Ali Rahjouei

## Introduction
Deoxyribonucleic acid (DNA) is a polymer composed of adenine (A) or thymine (T), cytosine (C) and guanine (G), a sugar called deoxyribose, and a phosphate group. Repeat of these unites through the entire genome generate codes essential for all cellular activities. This information can be read and translated into proteins as molecules with many different roles and functions. Just a small portion of genome consist of coding sequences and the rest of genome has structural and regulatory roles. Genomic duplications, deletions, rearrangements and mutations are the main source of genomic diversification during species evolution. The footprint of events can be observed as repetitive patterns through the entire genome. Under evolutionary pressure, some of these patterns or repeats remain intact which, can be recognized by different DNA-binding proteins and have a fundamental role in gene regulation and chromatin biology. Identifying DNA features is a difficult and resource consumable tasks.

Our recent advancement in unsupervised image classification methods inspired me to use these methods to identify genomic features by converting genetic information to arrays or figures. Then use the generated dataset to train a neural network and extract recognized genomic features and classify the segmented genomic by k-mean clustering. 

![alt text](https://db3pap006files.storage.live.com/y4m3maGE76ySwNzy9fb3_UsoEq2H5UCNwDLYmiAo_nSZK0sFMJcTeAioVNaZeCVR92HYEqG9ZSRZ3b40zWh-w1py-51R_V9m8LlfU8GOBy5wWLSw5rZqfRhkkt8-tuD7qYnwOmZu9qhPR3f_RHL7vhzFOi3BmL5gZkyOIIZ335vsqjYkUsXl1IkUQFkg925p2sd?width=1522&height=507&cropmode=none)

## Converting DNA sequences to image in R
1) The idea is to read the reference genome in fasta format (for example:https://ftp.ensembl.org/pub/release-108/fasta/drosophila_melanogaster/) and convert a, t, c and g to a vector (R) or list (Python) containing integers (1, 2, 3, 4).
2) Converting vector or list to array: array dimentions
3) Converting array to .png images.

Codes in R:
```
#loading library
library(seqinr)

#loading DNA sequence in fasta format
DF = read.fasta(file = "PATH/Drosophila_melanogaster.BDGP6.32.dna.toplevel.fa.gz")

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
- Cropping, resizing and bluring::

Coding in python:
```
# Loading packages
import cv2 as cv
import numpy as np
from matplotlib import pyplot as plt
#Loading image
img = cv.imread('PATH/bin10001.png')
```
## Improving Feature Visualization through Blurring Techniques 
Blurring can be a useful pre-processing technique for improving feature visualization in CNNs. By reducing fine details and emphasizing larger structures in an image, a CNN is better able to identify key features. However, in this specific project, I used blurring for a different purpose. In this project, blurring was used as a pre-processing technique on generated images from DNA sequences, where the combination of adjacent pixels created patterns that aid in identifying repeated sequences as a feature, thus helping in the analysis of genetic data.
```
# Cropping image
cropped_image = img[58:407, 59:450]
# Resizing image
resized_image = cv.resize(cropped_image,(200, 200))
# Changing color panel
image = cv.cvtColor(resized_image,cv.COLOR_BGR2RGB)
# Bluring image by cv.blur function
blur_image3 = cv.blur(image,(3,3))
blur_image5 = cv.blur(image,(5,5))
blur_image7 = cv.blur(image,(7,7))
blur_image9 = cv.blur(image,(9,9))
# Plotting blured image
plt.subplot(151),plt.imshow(image),plt.title('1')
plt.xticks([]), plt.yticks([])
plt.subplot(152),plt.imshow(blur_image3),plt.title('3')
plt.xticks([]), plt.yticks([])
plt.subplot(153),plt.imshow(blur_image5),plt.title('5')
plt.xticks([]), plt.yticks([])
plt.subplot(154),plt.imshow(blur_image7),plt.title('7')
plt.xticks([]), plt.yticks([])
plt.subplot(155),plt.imshow(blur_image9),plt.title('9')
plt.xticks([]), plt.yticks([])
plt.show()
```
![alt text](https://db3pap006files.storage.live.com/y4mfJo5n2a1L9iDKiNSrMpFbfDtj8ZYVLuWquqeC8vgSYdFq_kTMzQrZnm1DTh7W9EXg0EIbkQn_3eIk1XCDLCfgSlL8gSYmzXeOkYuHeaLGNrghCSSJNmfVjR_Ywg0ppC2E-HgG0RnCBuKWCR7VVG1WTABd2GXVShDU_pfGwkICnIwFZ5wRGEAjIECofH1wLbe?width=352&height=91&cropmode=none)

```
# Cropping image
cropped_image = img[58:407, 59:450]
# Resizing image
resized_image = cv.resize(cropped_image,(100, 100))
# Changing color panel
image = cv.cvtColor(resized_image,cv.COLOR_BGR2RGB)
# Bluring image by cv.GaussianBlur function
gaussianblur_image3 = cv.GaussianBlur(resized_image,(3,3),0)
gaussianblur_image5 = cv.GaussianBlur(resized_image,(5,5),0)
gaussianblur_image7 = cv.GaussianBlur(resized_image,(7,7),0)
gaussianblur_image9 = cv.GaussianBlur(resized_image,(9,9),0)
# Plotting blured image
plt.subplot(151),plt.imshow(image),plt.title('1')
plt.xticks([]), plt.yticks([])
plt.subplot(152),plt.imshow(gaussianblur_image3),plt.title('3')
plt.xticks([]), plt.yticks([])
plt.subplot(153),plt.imshow(gaussianblur_image5),plt.title('5')
plt.xticks([]), plt.yticks([])
plt.subplot(154),plt.imshow(gaussianblur_image7),plt.title('7')
plt.xticks([]), plt.yticks([])
plt.subplot(155),plt.imshow(gaussianblur_image9),plt.title('9')
plt.xticks([]), plt.yticks([])
plt.show()#
```
![alt text](https://db3pap006files.storage.live.com/y4mv2MJAfAEyrhRQqfs_WUqr4JjmwrL0z0A_wV3hK6G6PkBkmR5Xryw_lfgTXAA_mo67Fp9XNVCdi0EnVBpAn03SKMlXDCflliSEpetZ9ByR7SEYliba46Z1WDzMvm2Txe63uiFKDoTgonjhP_YclKAi14qpI2b_SwrLNubl-9bTFdZCc9h7KAIawIjdZYqs_bH?width=352&height=91&cropmode=none)

```
# Cropping image
cropped_image = img[58:407, 59:450]
# Resizing image
resized_image = cv.resize(cropped_image,(100, 100))
# Changing color panel
image = cv.cvtColor(resized_image,cv.COLOR_BGR2RGB)
# Bluring image by cv.medianBlur function
median_image3 = cv.medianBlur(resized_image,3)
median_image5 = cv.medianBlur(resized_image,5)
median_image7 = cv.medianBlur(resized_image,7)
median_image9 = cv.medianBlur(resized_image,9)
# Plotting blured image
plt.subplot(151),plt.imshow(image),plt.title('1')
plt.xticks([]), plt.yticks([])
plt.subplot(152),plt.imshow(median_image3),plt.title('3')
plt.xticks([]), plt.yticks([])
plt.subplot(153),plt.imshow(median_image5),plt.title('5')
plt.xticks([]), plt.yticks([])
plt.subplot(154),plt.imshow(median_image7),plt.title('7')
plt.xticks([]), plt.yticks([])
plt.subplot(155),plt.imshow(median_image9),plt.title('9')
plt.xticks([]), plt.yticks([])
plt.show()
```
![alt text](https://db3pap006files.storage.live.com/y4mcQnadOznKLVbZPOr-9wmSPBT0m8fKuBCP5oiTNZu4VTU-J0i7TnybnYa5ljiyVvhUTDMKnWyLhZmCQufsXmO94S33hr-86AeN0zAWeNljIHk0N0Xk5GqoMZKlrBC0M4nCN8M9PEkE81CIlsika6G7pYM6V20BbF0KvFAendwfY_zMyPU-XQK1kdVH6ysrxp1?width=352&height=91&cropmode=none)

## Projects:
#### Discovering features of DNA sequences: 
- Fixed frame: 
- Shifting frame:
#### Identifying DNA from different species:
#### Adding bigwig information on top of DNA sequence frame:
