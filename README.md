Log
================
``` sh
This log will document my workflow throughout the course as well as be a platform for Dr. Barshis to follow my progress.
```
## Created log repository:
<https://github.com/Getterrobog/Log.git>
I ran the commands Dan instructed.  I need to remember the following:
``` sh
git add README.md 
git commit -m "insert relevant comment"
git push -u origin main
```
### Day 2

Exercise day02
``` sh
/cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/Ivan/data/exercises
gunzip Exercise2.fasta.gz 
tar -zxvf Exercise2.fastq.tar.gz
grep -c \> Exercise2.fasta
There are 138 sequences in Exercise2.fasta
grep -c "@HS3" Exercise2.fastq
There are 61304 sequences in Exercise2.fastq
The total number of sequences is 138
The average sequence length is 126640
The total number of bases is 17476447
The minimum sequence length is 1122
The maximum sequence length is 562680
The N50 is 187037
Median Length = 92612
contigs < 150bp = 0
contigs >= 500bp = 138
contigs >= 1000bp = 138
contigs >= 2000bp = 135
```
Homework day02
```sh
pwd
/cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/Ivan/data

#!/bin/bash -l

#SBATCH -o ivancopylane02.txt
#SBATCH -n 1
#SBATCH --mail-user=ilope002@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Ivan_fastq_cp

cp /cm/shared/courses/dbarshis/21AdvGenomics/classdata/Astrangia_poculata/originalfastqs/HADB02*.fastq.gz /cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/Ivan/data/

sbatch FQCP.sh 
Submitted batch job 9270447

squeue -u ilope002
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
           9270431      main       sh ilope002  R      36:36      1 coreV2-25-072 
           9270447      main Ivan_fas ilope002  R       1:06      1 coreV2-25-072 

#!/bin/bash -l

#SBATCH -o ivangunziplane2.txt
#SBATCH -n 1
#SBATCH --mail-user=ilope002@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Ivan_fastq_gunzip

gunzip *.fastq.gz

sbatch FQgunzip.sh 
Submitted batch job 9270463

 squeue -u ilope002
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
           9270431      main       sh ilope002  R      54:38      1 coreV2-25-072 
           9270463      main Ivan_fas ilope002  R       0:38      1 coreV2-25-072
```
### Day 3

Exercise day03
```sh

```