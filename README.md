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
for i in *cutadapt.fastq; do fastqc $i; done

## Alignment of samples to reference
# Indexing the reference genome
bwa index ../../../Lin_ref_genome/GWHCAXI00000000.genome.fasta
# Alignments
bwa mem -t 4 -k 32 -M ../../Lin_ref_genome/GWHCAXI00000000.genome.fasta ../trimmed_data/CRR647019_f1.cutadapt.fastq ../trimmed_data/CRR647019_r2.cutadapt.fastq > acoma.sam
bwa mem -t 4 -k 32 -M ../../Lin_ref_genome/GWHCAXI00000000.genome.fasta ../trimmed_data/CRR647020_f1.cutadapt.fastq ../trimmed_data/CRR647020_r2.cutadapt.fastq > apalachee.sam
bwa mem -t 4 -k 32 -M ../../Lin_ref_genome/GWHCAXI00000000.genome.fasta ../trimmed_data/CRR647039_f1.cutadapt.fastq ../trimmed_data/CRR647039_r2.cutadapt.fastq > dallas.sam

## Clean up alignment data
# Remove duplicate reads
samtools rmdup acoma.sam acoma.rmdup.sam
samtools rmdup apalachee.sam apalachee.rmdup.sam
samtools rmdup dallas.sam dallas.rmdup.sam
# Convert sam files to bam files
samtools view -S -b acoma.rmdup.sam > acoma.bam; samtools sort acoma.bam > acoma.sorted.bam
samtools view -S -b apalachee.rmdup.sam > apalachee.bam; samtools sort apalachee.bam > apalachee.sorted.bam
samtools view -S -b dallas.rmdup.sam > dallas.bam; samtools sort dallas.bam > dallas.sorted.bam
