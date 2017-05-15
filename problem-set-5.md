# Chris Erickson HW5


# Overview
For this problem set you will write a series of python programs to perform
sequence analysis tasks. Please edit this document to include your answers and
include your scripts in your pull request.  

## Problem 1
Write a python program that reports the number of intervals for each
chromosome in a bed file. Write the program as a python script named
``problem_1.py``. 

Execute your program on the ``data-sets/bed/lamina.bed`` file:

```bash
python3 problem_1.py lamina.bed
```

Chris Erickson's problem_1 script
```python
  1 #! /usr/bin/env python3
  2 
  3 import sys
  4 from collections import Counter
  5 
  6 filename = sys.argv[1]
  7 print(filename)
  8 
  9 counts = Counter() #Dictionary will add value (interval) to each key
 10 #(chr) 
 11 #unique_chr = set() #start a set for each unique chr
 12 
 13 for line in open(filename):
 14     if line.startswith('#'):
 15         continue
 16     fields = line.split('\t')
 17     chrom = fields[0]
 18     counts[chrom] += 1
 19     
 20 for chrom, count in counts.items():
 21     print(chrom, count, sep = '\t')
 ```

Which chromosome has the most intervals?
``Chromosome 3``







## Problem 2
Write a python program that parses a fastq file and determines the total number
of ``C`` bases in the fastq. Write the program as a python script named
``problem_2.py``.

Execute your program on the ``data-sets/fastq/SP1.fq`` file.

Chris Erickson's problem_2 script
```python
  6 import sys
  7 filename = sys.argv[1]
  8 
  9 from collections import Counter
 10 bases = Counter()
 11 
 12 count = 0
 13 
 14 for line in open(filename):
 15     line = line.rstrip()
 16 
 17     if count == 0:
 18         count += 1
 19 
 20     elif count == 1:
 21         seq = line
 22         count += 1
 23 
 24     elif count == 2:
 25         count += 1
 26 
 27     elif count == 3:
 28         count = 0
 30
 34         for base in seq:
 35            bases[base] += 1
 36
 41 for key, value in bases.items():
 42     if key == "C":
 43         print(key, value)
 ```
 
```bash
python3 problem_2.py SP1.fq
```

What is the total number of ``C`` bases?
``2046``




## Problem 3: STILL WORKING ON #3
Write a python program that parses a fastq file and determines the most
common 6-mer (aka hexamer) sequence at the 5' and also the 3' end of each read. 
Write the program as a python script named ``problem_3.py``

What is the most common hexamer at the 5' end?
``write your answer here``

What is the most common hexamer at the 3' end?
``write your answer here``





## Problem 4:

The ``pysam`` package makes it easy to parse through a ``bam`` file
and extract important attributes. For this problem write a python program
named ``problem_4.py`` that parses a bam file and reports the following metrics: 
    
1. The total number of alignments on the positive and the negative strand. 
Use the Sam Flag attribute to determine if the alignment is on the forward or reverse
strand. 16 indicates an alignment on the reverse strand and 0 defines positive 
for this experiment. (hint: use the ``.flag`` attribute of an ``AlignedSegment``). More
information about these Flags in section 1.4 of the SAM [specification](http://samtools.github.io/hts-specs/SAMv1.pdf) 
    
2. The total number of alignments with no mismatches and the number of alignments
with more than zero mismatches. The NM tag records the edit distance between
the sequence and the reference. (hint: use the ``.get_tag()`` method to extract
the ``NM`` tag value). 

FYI to view the contents of a bam file as plain-text, use samtools view:

Chris Erickson's problem_4 script
```python
  3 import sys
  4 filename = sys.argv[1]
  5 
  6 from pysam import AlignmentFile
  7 bamfile = AlignmentFile(filename) 
  8 
  9 pos = 0
 10 neg = 0
 11 notmis = 0
 12 mis = 0
 13 
 14 for record in bamfile:
 15     if record.flag == 0:
 16         neg += 1
 17 
 18     elif record.flag == 16:
 19         pos += 1
 20 
 21 print(pos, neg)
 22 
 23 bamfile = AlignmentFile(filename)
 24 
 25 for record in bamfile:
 26     if record.get_tag('NM') == 0:
 27         notmis +=1
 28 
 29     elif record.get_tag('NM') > 0:
 30         mis += 1
 31 
 32 print(notmis, mis)
 ```

```bash
samtools view file.bam | less
```

How many positive strand aligned reads are there?
``27098``

How many negative strand aligned reads?
``26596``

How many alignments with no mismatches?
``42694``

How many alignments with more than zero mismatches?
``11000``

