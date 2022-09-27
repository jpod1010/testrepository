#setup environment for grabbing short read files, reference genome and dependencies 
#files are derived from  https://doi.org/10.1128/mBio.01501-16 paper describing short read files of B.anthracis metagenomic samples generated from from autopsy of corpses from Sverdlovsk 1979 outbreak, compared against B.anthracis Ames ancestor strain
#bbmap toolkit downloaded from https://sourceforge.net/projects/bbmap/files/latest/download and untar.gz'd
#sratoolskit downloaded from https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/3.0.0/sratoolkit.3.0.0-ubuntu64.tar.gz and untar.gz'd

conda create -n ginkgotest
conda install -c bioconda bowtie2
conda install -c bioconda fastqc
conda install -c bioconda samtools
conda install -c bioconda tabix
conda install -c bioconda ensembl-vep


#NCBI SRA file SRR2968141 was downloaded, Svd-2 HiSeq 21.RA93.38.4 lymph node sample, aligned against reference B.Anthracis strain 'Ames Ancestor' RefSeq GCF_000008445.1

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/008/445/GCF_000008445.1_ASM844v1/GCF_000008445.1_ASM844v1_genomic.gff.gz
gunzip GCF_000008445.1_ASM844v1/GCF_000008445.1_ASM844v1_genomic.gff.gz


wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/008/445/GCF_000008445.1_ASM844v1/GCF_000008445.1_ASM844v1_genomic.fna.gz
gunzip GCF_000008445.1_ASM844v1_genomic.fna.gz
bowtie2-build GCF_000008445.1_ASM844v1_genomic.fna Banthracis.index

prefetch SRR2968141
fastq-dump SRR2968141 --split-3
reformat.sh in=SRR2968141_1.fastq in2=SRR2968141_2.fastq out=SRR2968141_interleaved.fastq.gz




