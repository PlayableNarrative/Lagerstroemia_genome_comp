# Comparison of ABGs in Dallas Red, Apalachee, and Acoma

## Download raw sequencing data
### Acoma
wget https://download.cncb.ac.cn/gsa2/CRA009681/CRR647019/CRR647019_f1.fq.gz; wget https://download.cncb.ac.cn/gsa2/CRA009681/CRR647019/CRR647019_r2.fq.gz
### Apalachee
wget https://download.cncb.ac.cn/gsa2/CRA009681/CRR647020/CRR647020_f1.fq.gz; wget https://download.cncb.ac.cn/gsa2/CRA009681/CRR647020/CRR647020_r2.fq.gz
### Dallas Red
wget https://download.cncb.ac.cn/gsa2/CRA009681/CRR647039/CRR647039_f1.fq.gz; wget https://download.cncb.ac.cn/gsa2/CRA009681/CRR647039/CRR647039_r2.fq.gz
### Unzip
for i in CRR6470*; do gunzip $i; done

## Trimming and QC
### Cutadapt for each sample using Illumina adaptors
cutadapt -a CTGTCTCTTATACACATCT -A CTGTCTCTTATACACATCT -o CRR647039_f1.cutadapt.fastq -p CRR647039_r2.cutadapt.fastq CRR647039_f1.fq CRR647039_r2.fq 
### Check quality with FastQC
