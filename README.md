# DeepCAT<img  align="left" src="https://github.com/s175573/DeepCAT/blob/master/Figures/Cat.png" width="35" height="45" > 

### Deep CNN Model for Cancer Associated TCRs

DeepCAT is a computational method based on convolutional neural network to exclusively identify cancer-associated beta chain TCR hypervariable CDR3 sequences. The input data were generated from tumor RNA-seq data and TCR repertoire sequencing data of healthy donors. Users do not need to perform training or evaluation. Instead, users can directly apply the PredictCancer function in the package, after downloading the CHKP folder. 

### Standard pipeline of using DeepCAT



 1. Clone github repository on your own machine in a desired folder

&nbsp; &nbsp; &nbsp;&nbsp;
    In Terminal:

```bash
  git clone https://github.com/HUNNNGRY/DeepCAT.git
```

 2. Go to DeepCAT folder and unzip DeepCAT_CHKP.zip file with pre-trained model 
   
```bash
  cd DeepCAT
  unzip DeepCAT_CHKP.zip 
```

 3. Running the DeepCAT requires python3, biopython, tensorflow version 1.4 and matplotlib packages to be installed. If they are not installed on your machine, please run the command:
 
```bash
  conda create -n DeepCAT python=3.6 tensorflow==1.14 biopython matplotlib scikit-learn
```

 4. Using raw TCR repertoire sequencing data from adaptativebiotech database  

-t: input dir

```bash
  bash  Script_DeepCAT.sh -t SampleData/Control    
  bash  Script_DeepCAT.sh -t SampleData/Cancer
```

&nbsp; &nbsp; &nbsp;&nbsp;
DeepCAT will output two files, Cancer_score_Control.txt and Cancer_score_Cancer.txt in current DeepCAT dir,like this:

```bash
  $ head Cancer_score_Control.txt
  TestReal-HIP09051.tsv_ClusteredCDR3s_7.5.txt	0.22574785
  TestReal-HIP09559.tsv_ClusteredCDR3s_7.5.txt	0.16407683
  TestReal-HIP09364.tsv_ClusteredCDR3s_7.5.txt	0.21816333
  TestReal-HIP09062.tsv_ClusteredCDR3s_7.5.txt	0.17059885
  TestReal-HIP09190.tsv_ClusteredCDR3s_7.5.txt	0.17043449
  TestReal-HIP09022.tsv_ClusteredCDR3s_7.5.txt	0.16097252
  TestReal-HIP09029.tsv_ClusteredCDR3s_7.5.txt	0.172395
  TestReal-HIP09159.tsv_ClusteredCDR3s_7.5.txt	0.17491624
  TestReal-HIP09775.tsv_ClusteredCDR3s_7.5.txt	0.21496484
  TestReal-HIP10377.tsv_ClusteredCDR3s_7.5.txt	0.19861585
```

&nbsp; &nbsp; &nbsp;&nbsp; 
where first column contains name of the input file, second column is mean cancer score for all sequences in corresponding input file.<br />

&nbsp; &nbsp; &nbsp;&nbsp;
Let’s make boxplots with cancer scores and ROC curve for early-stage breast cancer patients (16 samples) and healthy donors (30 samples).

<p float="left">
  <img src="Figures/Figure_Cancer_score.png" width="930" height="450"/>
</p>


To date, cancer score estimation is the average of DeepCAT output probabilities, and is intended to evaluate only for unsorted PBMC samples from healthy individuals or untreated cancer patients. _Application of cancer scores on flow-sorted T cells, TCR-seq data profiled using a different platform, patient samples with poor quality (low total template counts), or patients with chronic inflammatory conditions may lead to undesirable results._ These results or observations cannot be compared to the data generated in the DeepCAT publication (currently in submission). 


### Docker image for DeepCAT  (optional)
The Docker image can be downloaded at https://hub.docker.com/r/deepcat/cancer_associated_tcr
```bash
docker pull deepcat/cancer_associated_tcr:1
```

To save image, type in Terminal:
```bash
docker save deepcat/cancer_associated_tcr > deep_cat.tar
docker load --input deep_cat.tar
```
To run docker image:
```bash
docker run -v $(pwd)/Results:/Results deepcat/cancer_associated_tcr:1
```
