# Gene Repository
Hello. This is a step by step process to utilize the tools, commands and programs that I used to gather the data needed for the Bio 312 research paper on the evolution of a gene that we were given through deuterostomes. Enjoy :)

# Make a directory
First, make a directory in which we can store all of our computations for CDC40 using the following command:
```bash
mkdir /home/ec2-user/CDC40
```
This command will make a directory file under the ec2-user folder in VScode. You can change the specific location where you want to put this file too.

# Downloading CDC40 gene from NCBI BLAST database

First we will download the CDC40 gene as a query sequence. Download the sequence using the following command:
```bash
ncbi-acc-download -F fasta -m protein NP_056480.1 
```

This command essentially downloads the gene sequence for CDC40 from the NCBI database and turns it into a fasta file

## Performing the BLAST search

To perfrom a blast search using our query protein (CDC40) use the following command
```bash
blastp -db ../allprotein.fas -query NP_056480.1.fa -outfmt 0 -max_hsps 1 -out CDC40.blastp.typical.out
```

# Perform BLAST search adnd request tabular output
The following command will create a more detailed output of the dame analysis performed prior. 
```bash
blastp -db ../allprotein.fas -query NP_056480.1.fa  -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out CDC40.blastp.detail.out
```
## Filtering the BLAST output for high-scoring putative homologs
Next, we'll filter the homologs we've accumulated from the BLAST search such that high-scoring matches are included. In this case the command I used utilized an e-value of 1e-35, which will take into account of homologs in extreme close match to the CDC40 gene copy found in H.sapiens:
```bash
awk '{if ($6< 1e-35)print $1 }' globins.blastp.detail.out > CDC40.blastp.detail.filtered.out
```

To determine the number of paralogs, which was ultized in Table 1 of my paper, I input the following command:
```bash
grep -o -E "^[A-Z]\.[a-z]+" CDC40.blastp.detail.filtered.out  | sort | uniq -c
```
This command counts the number of paralogs found in each species after filting to include high-scoring matches from the BLAST search.
