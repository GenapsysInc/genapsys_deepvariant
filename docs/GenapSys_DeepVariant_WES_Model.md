## GenapSys<sup>TM</sup> DeepVariant Whole Exome Sequencing (WES) Model

The GenapSys team has developed a Germline variant calling pipeline for the GenapSys sequencing technology based on Google’s DeepVariant model. This pipeline is available for use on GenHub<sup>TM</sup> (GenapSys Cloud-based analysis platform).

![Pipeline Image](/images/pipeline.png)
===

We carried out whole exome sequencing of DNA from the extremely well characterized human cell line NA12878. NA12878 is a lymphoblastoid cell line (LCL) prepared from a normal female Caucasian participant of the international HapMap project. This DNA sample has served as a human reference standard for the Genome in a Bottle (GIAB v3.3.2) Consortium and has been well characterized using a variety of different genomic technologies. Targeted sequencing libraries made from NA12878 genomic DNA were generated via a probe-based capture method using the IDT xGEN Exome Research Panel (v1.0). 

Sequencing reads were aligned to the hg38 reference genome using BWA MEM (v0.7.17). Google's pre-trained DeepVariant model (v0.9.0) for Germline variant calling was taken as base and further trained on GenapSys data to yield the model provided as part of this report. In training and testing the model, a lower bound cutoff allele frequency of 12% was considered for the variants. The GIAB high confidence variants within the IDT xGen Exome panel on all chromosomes were used as the ground truth. The aligned bam file and the variant call vcf files can be downloaded from [here](https://Addlink.com). 

Here we summarize the Germline variant calling performance of the GenapSys DeepVariant model tested on GenapSys WES dataset (100X coverage).

Type | Technology | True positives | False negatives | False positives | Recall | Precision | F1-Score
:--: | :--------: | :------------: | :-------------: | :-------------: | :----: | :-------: | :-------------:
SNP | GenapSys | 20929 | 170 | 8 | 0.992 | 1.00 | 0.996
INDEL | GenapSys | 664 | 74 | 63 | 0.900 | 0.913 | 0.906



# How to run:

# Install Docker.
Please follow the instructions on the Docker’s website.
https://docs.docker.com/install/linux/docker-ce/ubuntu/

# Pull the Docker image.
docker pull genapsysinc/genapsys_deepvariant:0.9.0.1

# Run GenapSys-DeepVariant
docker run \
  -v "YOUR_INPUT_DIR":"/input" \
  -v "YOUR_OUTPUT_DIR:/output" \
  genapsysinc/genapsys_deepvariant:0.9.0.1 \
  /opt/run_genapsys_deepvariant \
  --model_type=WES \
  --ref=/input/YOUR_REF \
  --regions=/input/YOUR_BED \
  --reads=/input/YOUR_INPUT_BAM \
  --output_vcf=/output/YOUR_OUTPUT_VCF \
  --num_shards=$(nproc)  # number of cores to run make_examples



