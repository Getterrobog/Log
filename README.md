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
###Day 5

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
```
###Day 6

Homework day 06
```sh
salloc
salloc: Pending job allocation 9272861
salloc: job 9272861 queued and waiting for resources
salloc: job 9272861 has been allocated resources
salloc: Granted job allocation 9272861

cda
cd data/fastq/QCFastqs/
mkdir Trinity
mkdir Trinity/trinity_out_dir
cd Trinity/trinity_out_dir
cp cp /cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/kparker/data/fastq/QCFastqs/Trinity/djb_trinity_out/Trinity.fasta .

/cm/shared/apps/trinity/2.0.6/util/TrinityStats.pl Trinity.fasta 
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LANG = "C.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").


################################
## Counts of transcripts, etc.
################################
Total trinity 'genes':	34661
Total trinity transcripts:	36565
Percent GC: 44.57

########################################
Stats based on ALL transcript contigs:
########################################

	Contig N10: 1476
	Contig N20: 874
	Contig N30: 619
	Contig N40: 476
	Contig N50: 388

	Median contig length: 285
	Average contig: 388.75
	Total assembled bases: 14214539


#####################################################
## Stats based on ONLY LONGEST ISOFORM per 'GENE':
#####################################################

	Contig N10: 1217
	Contig N20: 746
	Contig N30: 554
	Contig N40: 440
	Contig N50: 368

	Median contig length: 282
	Average contig: 371.25
	Total assembled bases: 12867801

cd ../../
pwd
/cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/Ivan/data/fastq/QCFastqs
less ivanbowtie2.txt 
grep "overall alignment rate" ivanbowtie2.txt| cut -d "%" -f1
1.91
1.45
1.09
2.03
3.06
2.21
2.05
2.25
2.42
2.35
2.15
1.50
2.50
1.67
35.61
1.68
Average 4.12%

grep "aligned exactly 1 time" ivanbowtie2.txt
0.86
0.6
0.68
1.23
1.44
1.36
1.13
1.26
1.04
1.11
1.24
1.01
1.37
1.06
0.91
1.27
Average 1.098125%

grep "aligned exactly 1 time" fastq/QCFastqs/ivanbowtie2.txt | cut -d "(" -f1
50875 
62969 
94235 
167093 
54548 
197608 
158279 
173486 
87780 
163906 
93271 
93998 
172337 
246957 
136 
2129737 
Average number of reads 246700.9375

grep "aligned >1 times" ivanbowtie2.txt
1.05
0.85
0.41
0.79
1.62
0.85
0.92
0.99
1.38
1.24
0.91
0.49
1.14
0.61
34.7
0.41
Average 3.0225%

nano alignstats.txt
Lane02_irl	4.120625	1.098125	246700.9375 3.0225
[ilope002@coreV2-22-007 QCFastqs]$ cat alignstats.txt >> /cm/shared/courses/dbarshis/21AdvGenomics/classdata/Astrangia_poculata/alignmentstatstable.txt

cd ../../
mkdir testassembly
mv fastq/QCFastqs/Trinity/trinity_out_dir/Trinity.fasta ./testassembly/

#!/bin/bash -l

#SBATCH -o ivancleaner.txt
#SBATCH -n 1
#SBATCH --mail-user=ilope002@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Ivan_cleaner

rm -r /cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/Ivan/data/fastq/QCFastqs/Trinity
rm -r /cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/Ivan/data/fastq/originalfastqs
rm -r /cm/shared/courses/dbarshis/21AdvGenomics/sandboxes/Ivan/data/fastq/filteringstats

sbatch ../scripts/cleaner.sh
Submitted batch job 9272877

#!/bin/bash -l

#SBATCH -o ivanblaster.txt
#SBATCH -n 6
#SBATCH --mail-user=ilope002@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Ivan_blaster

enable_lmod
module load container_env blast
blastx -query Trinity.fasta -db /cm/shared/apps/blast/databases/uniprot_sprot_Sep2018 -out blastx.outfmt6 \ -evalue 1e-20 -num_threads 6 -max_target_seqs 1 -outfmt 6

cd testassembly/

sbatch ../../scripts/blaster.sh 
Submitted batch job 9272880

squeue -u ilope002
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
           9272884      main Ivan_bla ilope002 PD       0:00      1 (Priority) 
           9272861      main       sh ilope002  R    3:40:49      1 coreV2-22-007 

Job failed
The out file stated -evalue is an error, but man states this is a proper flag.  I 
readjusted the script.  I am not sure why the \ character is necessary, but the 
script failed as I wrote it.  I will try \n and tab after \, and then removing 
the \ and put all the flags on one line.
The script worked

cda
cd data/fastq/QCfastqs
head RI_B_02_14_clippedtrimmed.fastq.sam
grep -v '@' RI_B_02_14_clippedtrimmed.fastq.sam | head

All the lines in the header start with @

salloc
salloc: Pending job allocation 9273035
salloc: job 9273035 queued and waiting for resources
salloc: job 9273035 has been allocated resources
salloc: Granted job allocation 9273035

/cm/shared/courses/dbarshis/21AdvGenomics/scripts/get_explain_sam_flags_advbioinf.py *14_clippedtrimmed.fastq.sam

RI_B_02_14_clippedtrimmed.fastq.sam
['0', '272', '256', '16']
0 :
272 :
	read reverse strand
	not primary alignment
256 :
	not primary alignment
16 :
	read reverse strand
RI_W_02_14_clippedtrimmed.fastq.sam
['0', '272', '256', '16']
0 :
272 :
	read reverse strand
	not primary alignment
256 :
	not primary alignment
16 :
	read reverse strand
VA_B_02_14_clippedtrimmed.fastq.sam
['0', '272', '256', '16']
0 :
272 :
	read reverse strand
	not primary alignment
256 :
	not primary alignment
16 :
	read reverse strand
VA_W_02_14_clippedtrimmed.fastq.sam
['0', '272', '256', '16']
0 :
272 :
	read reverse strand
	not primary alignment
256 :
	not primary alignment
16 :
	read reverse strand

cd ../../../scripts/
nano Baminator.sh

#!/bin/bash -l

#SBATCH -o ivanBam.txt
#SBATCH -n 1
#SBATCH --mail-user=ilope002@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Ivan_Baminator

enable_lmod
module load samtools/1
for i in *.sam; do `samtools view -bS $i > ${i%.sam}_UNSORTED.bam`; done

for i in *UNSORTED.bam; do samtools sort $i > ${i%_UNSORTED.bam}.bam
samtools index ${i%_UNSORTED.bam}.bam
done

cd ../data/fastq/QCFastqs/

sbatch ../../../scripts/Baminator.sh 
Submitted batch job 9273040

squeue -u ilope002
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
           9273040      main Ivan_Bam ilope002 PD       0:00      1 (Priority) 
           9273028      main Ivan_bla ilope002  R    2:06:45      1 coreV1-22-005 
           9273035      main       sh ilope002  R      25:43      1 coreV1-22-003 


10- we need to run the read sorting step required for SNP calling, so if you have time, set up and run the following script on your .sam files to finish before Wednesday:

```
