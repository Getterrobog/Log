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

Homework day03
```sh
pwd
/cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/Ivan
cp -r /cm/shared/courses/dbarshis/21AdvGenomics/assignments_exercises/day03 ./
cd data
mkdir fastq
cd fastq
mv ../*.fastq ./
cp ./day03/renamingtable_complete.txt ./data/fastq/
cp /cm/shared/courses/dbarshis/21AdvGenomics/scripts/renamer_advbioinf.py ./scripts 
cp ../../scripts/renamer_advbioinf.py .
less renamer_advbioinf.py
python renamer_advbioinf.py renamingtable_complete.txt
output verified
nano renamer_advbioinf.py 
ls
RI_B_02_14.fastq   RI_W_02_22.fastq   VA_W_02_14.fastq
RI_B_02_18.fastq   RI_W_09_SNP.fastq  VA_W_02_18.fastq
RI_B_02_22.fastq   VA_B_02_14.fastq   VA_W_02_22.fastq
RI_B_09_SNP.fastq  VA_B_02_18.fastq   VA_W_09_SNP.fastq
RI_W_02_14.fastq   VA_B_02_22.fastq   renamer_advbioinf.py
RI_W_02_18.fastq   VA_B_08_SNP.fastq  renamingtable_complete.txt

cp /cm/shared/courses/dbarshis/21AdvGenomics/scripts/Trimclipfilterstatsbatch_advbioinf.py ../../scripts/
less ../../scripts/Trimclipfilterstatsbatch_advbioinf.py
cp /cm/shared/courses/dbarshis/21AdvGenomics/assignments_exercises/day03/adapterlist_advbioinf.txt . 

nano Trimclipfilter.sh

#!/bin/bash -l

#SBATCH -o ivanTrimclipfilter.txt
#SBATCH -n 1
#SBATCH --mail-user=ilope002@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Ivan_Trimclipfilter

python Trimclipfilterstatsbatch_advbioinf.py adapterlist_advbioinf.txt *.fastq

cp ../../scripts/Trimclipfilterstatsbatch_advbioinf.py .
Trimclipfilter.sh 
Submitted batch job 9270553
```
### Day 4
```sh
I created an alias (cda) to cd to my sandbox
```
Homework day04
```sh
cda
cd data/fastq/filteringstats/
pwd
/cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/Ivan/data/fastq/filteringstats
salloc
/cm/shared/courses/dbarshis/21AdvGenomics/scripts/Schafran_trimstatstable_advbioinf_clippedtrimmed.py -h
python /cm/shared/courses/dbarshis/21AdvGenomics/scripts/Schafran_trimstatstable_advbioinf_clippedtrimmed.py trimclipstats.txt Ivan_trimclipstatsout.txt
tail -n +2 YOURNAME_trimclipstatsout.txt >> /cm/shared/courses/dbarshis/21AdvGenomics/classdata/Astrangia_poculata/Fulltrimclipstatstable.txt
cd ../QCFastqs/

#!/bin/bash -l

#SBATCH -o ivanbowtie2.txt
#SBATCH -n 1
#SBATCH --mail-user=ilope002@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=JOBNAME

module load bowtie2/2.4
for i in *_clippedtrimmed.fastq; do bowtie2 --rg-id ${i%_clippedtrimmed.fastq} \
--rg SM:${i%_clippedtrimmed.fastq} \
--very-sensitive -x /cm/shared/courses/dbarshis/21AdvGenomics/classdata/Astrangia_poculata/refassembly/Apoc_hostsym -U $i \
> ${i%_clippedtrimmedfilterd.fastq}.sam --no-unal -k 5; done

sbatch bowtie2.sh 
Submitted batch job 9271111

squeue -u ilope002
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
           9271111      main  JOBNAME ilope002  R       0:09      1 coreV2-25-007 
           9271099      main       sh ilope002  R    1:03:39      1 coreV3-23-033 
```
Homework day 05
```sh
Katie and Ivan 
Katie mkdir QCFastqs/Trinity
cp *.fastq ../../../../kparker/data/fastq/QCFastqs/Trinity/
cd ../../../../kparker/data/fastq/QCFastqs/Trinity/
ls *.fastq | head -32 > 32fileslist.txt
nano Trinitybash.sh

#!/bin/bash -l

#SBATCH -o Katie_Ivan_Trinity.txt
#SBATCH -n 32
#SBATCH -p himem
#SBATCH --mail-user=ilope002@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=KatieIvanTrinity

enable_lmod
module load container_env trinity

crun Trinity --seqType fq --max_memory 768G --normalize_reads --single RI_B_02_14_clippedtrimmed.fastq,RI_B_02_18_clippedtrimmed.fastq,RI_B_02_22_clippedtrimmed.fastq,RI_B_03_14_clippedtrimmed.fastq,RI_B_03_18_clippedtrimmed.fastq,RI_B_03_22_clippedtrimmed.fastq,RI_B_09_SNP_clippedtrimmed.fastq,RI_B_10_SNP_clippedtrimmed.fastq,RI_W_02_14_clippedtrimmed.fastq,RI_W_02_18_clippedtrimmed.fastq,RI_W_02_22_clippedtrimmed.fastq,RI_W_03_14_clippedtrimmed.fastq,RI_W_03_18_clippedtrimmed.fastq,RI_W_03_22_clippedtrimmed.fastq,RI_W_09_SNP_clippedtrimmed.fastq,RI_W_10_SNP_clippedtrimmed.fastq,VA_B_02_14_clippedtrimmed.fastq,VA_B_02_18_clippedtrimmed.fastq,VA_B_02_22_clippedtrimmed.fastq,VA_B_03_14_clippedtrimmed.fastq,VA_B_03_18_clippedtrimmed.fastq,VA_B_03_22_clippedtrimmed.fastq,VA_B_08_SNP_clippedtrimmed.fastq,VA_B_10_SNP_clippedtrimmed.fastq,VA_W_02_14_clippedtrimmed.fastq,VA_W_02_18_clippedtrimmed.fastq,VA_W_02_22_clippedtrimmed.fastq,VA_W_03_14_clippedtrimmed.fastq,VA_W_03_18_clippedtrimmed.fastq,VA_W_03_22_clippedtrimmed.fastq,VA_W_09_SNP_clippedtrimmed.fastq,VA_W_10_SNP_clippedtrimmed.fastq --CPU 32

Katie salloc
salloc: Pending job allocation 9272354
salloc: job 9272354 queued and waiting for resources
salloc: job 9272354 has been allocated resources
salloc: Granted job allocation 9272354
[kpark049@coreV2-22-007 Trinity]$ sbatch Trinitybash.sh
Submitted batch job 9272355

