# Protein Prediction in BRAKER3

[Galaxy](https://usegalaxy.eu) hosts multiple bioinformatics tools that are run from the GUI rather than the command line in Terminal. You can register for a free online account with 250 GB of output storage.


BRAKER3 uses RNA-Seq and/or predicted protein databases to guide protein prediction from a genome in .fna or .fasta format. RNA-Seq data are submitted as BAM files (very large) while predicted protein files are submitted as .fa or .fasta files obtained from NCBI or Uniprot (preferred) which are much smaller.


These instructions predict and then combine exons from your genome of interest into likely genes based on a guiding high-quality genome, convert those genes into protein sequences, and then assess the quality of those protein predictions. Instructions draw heavily from [Galaxy’s BRAKER3 tutorial](https://training.galaxyproject.org/training-material/topics/genome-annotation/tutorials/braker3/tutorial.html) with modifications.


## Procedure

### Data Set Up

1. Log in to your [Galaxy account](https://usegalaxy.eu).
   
2. Open the sequences headers in your genome file to be converted to proteins. Use Find-Replace to convert all spaces, colons, commas, etc. to underscores. For instance, " ", ": ", and ", " should be replaced with "_" so that "apple banana", "apple: banana", and "apple, banana" become "apple_banana".

3. Check your genome file to be converted to proteins to see if it is soft-masked. Open the file and see if some of the nucleotides are lower-case; if so, your file is soft-masked. 

4. If your genome file to be converted to proteins is not soft-masked, [follow these instructions](https://training.galaxyproject.org/training-material/topics/genome-annotation/tutorials/repeatmasker/tutorial.html) for Red or RepeatMasker tools. If you had to soft-mask the genome, be sure to use this new file in subsequent steps.

5. Click on Upload in the top left corner.

6. Drag your genome file to be converted to proteins and the reference RNA-Seq and/or predicted protein files into the upload pop-up window. Multiple files can be uploaded at once. Click Start. Then click Close.


### BRAKER3
1. In the Tools column (left half of screen), type Braker3 into the search bar. Click on BRAKER3 genome annotation. This opens the BRAKER3 tool on the right half of the screen.

2. Set the following:

a. Assembly to annotate = your genome file to be converted to proteins (.fna or .fasta file type). Remember to use the soft-masked version to improve protein prediction quality.

b. Genome sequence is soft-masked = Yes

c. RNA-seq mapped to genome to train Augustus/GeneMark = reference RNA-Seq.bam file; this is optional

d. Proteins to map to genome = reference predicted protein file from Uniprot (preferred) or NCBI as uniprot.fasta

e. Fungal genome = ignore if not working with fungal genome

f. Augustus settings = default

g. Advanced settings = default

h. Output format = GFF3

i. Click Run Tool at the bottom of the settings.

3. BRAKER3 could take anywhere from a few hours to 2-3 days to run depending on genome size and reference database quality.

4. To download the completed GFF file, click on the Save icon (floppy disk) in the lower left corner of the green box for that analysis (green = successfully completed) on the right side of the screen.

5. Errors in the analysis are indicated by a red box for that analysis. To see the error(s) information, click on the View symbol that looks like an eye inside box corners in the red box for that analysis. Then click on the Error icon that looks like a person with an ‘i’ on their abdomen. Galaxy includes an error wizard that will analyze error codes and give a layman’s description of the error plus possible fixes.


### GFFread

1. Convert the GFF file to CDS and protein sequences using the GFFread tool. Search for this tool the same way as you found BRAKER3 in step 6.

2. Set the following:

a. Input BED, GTF, or GFF3 feature file = the output from BRAKER3. This can be chosen from the dropdown directly in Galaxy (preferred as .gff uploads seem to be read by Galaxy as .bed files and so convert exons to CDSs incorrectly). Or you can upload the GFF file you downloaded in step 10.

b. Reference genome = From your history

c. Genome reference fasta = your genome to be converted to predicted proteins, which you already uploaded

d. Select fasta outputs = 
Fasta file with spliced exons for each GFF transcript (-w);
Protein fasta file with the translation of CDS for each record (-y);
For protein fasta: use (*) instead of (.) as stop codon translation (-S);

e. Full GFF attribute preservation = Yes

f. Decode URL encoded characters within attributes = Yes

g. Warn about duplicate transcript IDs and other potential problems with the given GTF/GFF records = Yes

3. Click Run Tool at the bottom of the settings.

4. Download the nucleotide and protein files as in step 10 for the GFF file output from BRAKER3.


### BUSCO

1. Evaluate the quality of the gene/protein model prediction for your genome using BUSCO ([Benchmarking Universal Single-Copy Orthologs[(https://watermark02.silverchair.com/bioinformatics_31_19_3210.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAA38wggN7BgkqhkiG9w0BBwagggNsMIIDaAIBADCCA2EGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMibWOgfi99KTCjhjaAgEQgIIDMpE29cO990Y7bRApuZqE2mCsCE-q5APcYb-1t-4G79TpRXxKm7RzyLmF-yEwKIW2XquNbPbPIa4KpKYHGck2i6JAu0ClE5EsGETpvOeZ3X3CND763bEVKuEgAF7h_BR0DEyP6oA9heCyFMq0NAugZwX-hcQs6gfvlXPegOkvLQ5rgZOnMC5R16hWFsCX3Z4qb20-IwjhkWi7ULt-LZ-yF5mdzn6C4BonxApEKcYt9ngK6zerKa6CwuM_9vPusiilLZZmIXiaSM1cGt7lOE-f0dhx3ieBbmMjPnZGZhmxbG1Eif0hfVL2sk5uAYkUkRlBxnCIAwrxqkvgKNy5nbdz8xJIxne1rJLtYMRNoN37oa7Lb5gC1ymHcjVRwP5Z0rmPHkx0E095pupIu7oflrC8e9Qkm3Ej0LV80hxsHAvCXX_NNWFMGTUaDvzf8ow5siiheftAbEwIoRhviXhhYRhAgBkWIJ16rER0rrEPB-vae849XbH33OrAG5PxhF4b9ZjdzcpfEA3dV2jFfx-T_U7kGKskzn2gep4xuhiYIYEy_UpbpdrcFtimdj_8cbnILCHbJ4LcY5rVPrj45IpMIaV7N985pkUESWA-ydMDvEkIqL-8K2ykVStR9os9-Lwd7hAKq1JFGgZ75_s4v8YFueHDTyJDbTAN0gwdpcyh79i29d9fCAQcSgnmy4Px9clAmbSISLIFwfqDmudwxW4Vv6LA7t0W83x_Zl6tn9pOqJOzSAE8CjmJNs92ypMDbnZUbiVXks7pIhJlnx8Ebe1N6uELlsou0Bhb8MLWcRhwDSk6WlfGcRC_5J2HT3szNkJhPdD4HqtsfS_TXVEg1t2QLhjqAvU63kwm8xMNnSw75M6dYaElLA_yUHz2jEIqJaMrPj_Qqk2rFw223dqhxEUPOCzBEZxW7CFTNODh6fVNyoDAvGh8UnJGudHmR6XcqaqW8nMC8tJ6c3Udr4pwfmTwcTAw4l1DpHVT4DGV9QK_Nc27lZJWWX4H63pTznR0BTm6wsKKRnlWEx0s0Mf_-Ew9WcujLY-NziuBUN9j7JvGftORiuPGxtqLHq9_jA8-6qCDEDWlhEsh)). Search for the BUSCO tool the same way as you found BRAKER3 in steps 6 and 12.

2. Set the following:

a. Sequences to analyze = the output from GFFread. This can be chosen from the dropdown directly in Galaxy. Or you can upload the pep.fa or fasta file you downloaded in step 15.

b. Cached database with lineage = Eukaryotes (DATE) from the dropdown

c. Mode = annotated gene sets (protein)

d. Auto-detect or select lineage = select lineage

e. Auto-lineage group = Eukaryotes (--auto-lineage-euk)

f. Which outputs should be generated =

g. Short summary text (default), at a minimum this one should be selected. If desired, any of the other files can be chosen:
Summary image and List with missing IDs will be small files; gff, Protein sequences, and Nucleotide sequences will be MB in size

h. All other settings should be default

3. Click Run Tool at the bottom of the settings.

4. High quality predictions for eukoryotes find complete versions of >90% of the reference orthologs
