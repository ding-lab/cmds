Project: CMDS - Cohort DNA Copy Number Analysis

Description: A population-based method for DNA copy number analysis: recurrent copy number
aberration identification in multiple samples (with no need of single-sample calling). 
Developed for a quick analysis of high resolution and large population data.

Website: http://cmds.sourceforge.net/

Email: qunyuan@dsgmail.wustl.edu

This release is fully-functional, and has been published:
Zhang et al. (2009) CMDS: a population-based method for identifying 
recurrent DNA copy number aberrations in cancer from high-resolution data.
Bioinformatics 26(4):464-469.

***********************
CMDS C version 1.0
**********************

By Xiaoqi Shi & Qunyuan Zhang, Feb 2010

Usage: cnv cmds [options] [infile] [outfile]

Optional Arguments:
-h if infile contains one line of header please indicate here
-w INT block size [defaul=50]
-c float cutoff value [defaul=10.0]
-s INT step size [defaul=1]
-m STRING missing value represented in infile[defaul=-999]

Example:

./cnv cmds -h -w 20 -s 1 -c 10 exampledata.txt example.out

Output file example.out contain 8 columns below:

1:chromosome ID
2:start position of a window
3:end position of a window
4:middle position of a window
5:CN mean of a window
6: the RCNA score (z) for the middle position of a window
7:SD of z
8:p-value of the RCNA test for the middle position

The executable file "cnv" was compiled on our i686 Linux machine, you may need to compile it yourself following the steps below:

1. copy the source code fold "cmds" to your machine;

2. run the two command lines under the dir cmds/src/

gcc -c -lz -lm cmds.c CNSdepth.c cnvhmm.c ForwardBackward.c hmm.h hmmutils.c nrutil.c nrutil.h statis.hh viterbi.c

gcc -o cnv cmds.o CNSdepth.o cnvhmm.o ForwardBackward.o hmmutils.o nrutil.o viterbi.o -lz -lm

An executable file "cnv" should be generated, then type "cnv cmds" to see the options and refer to the example dir for the data format.
 

Pleae be aware that our current program limits the number of characters per input line to 9999, If you want to increase it, please locate and change the line below in cmds/src/cdms.c file and re-complile it

char line [ 9999 ]; /* or other suitable maximum line size */

If you want to change the source code, please start from the main entrance file cmds/src/cnvhmm.c

Reference Online:

Abstract: http://bioinformatics.oxfordjournals.org/cgi/content/abstract/btp708?ijkey=Z6cxCrjhtCwK2dE&keytype=ref

PDF: http://bioinformatics.oxfordjournals.org/cgi/reprint/btp708?ijkey=Z6cxCrjhtCwK2dE&keytype=ref

Contact: Qunyuan Zhang

E-mail: qunyuan@wustl.edu


**********************
CMDS R version 1.0 
*********************

By Qunyuan Zhang (qunyuan@wustl.edu), Jan, 2010

To run CMDS, please follow the steps below:

1)Create a working DIR, download CMDS and unzip it, copy cmds.R and cmds_lib.R into it. 

2)Creat a data DIR, put copy number data file(s) in it.
Don't include any other files in the data DIR.
Each data file can be for either a chromosome or a chromosomal arm,
TAB delimited, with the first row listing the column names (please refer to exampledata). Missing value=NA.
The CN values can be raw intensity ratios or log2 ratios (between tumor/normal), or smoothed/segmented/estimated copy number data. 

3)Modify the options of the R function cmds.focal.test() in the file cmds.R, which may looks like
--------------------------
cmds.focal.test(
data.dir="exampledata",
wsize=30,
wstep=1,
analysis.ID=run.ID,
chr.colname="chromosome",
pos.colname="position", 
plot.dir="cmds_plot",
result.dir="cmds_test") 
-------------------------

[Details of options] 
data.dir: data directory. 
wsize: small wsize increases sensitivity and noise; large wsize reduces sensitivity and noise. 
wstep: large wstep reduces computer time and resolution. 
analysis.ID: =NA will analyze all data files; =i will analyze the i-th data file (i=1,2,3,...).
chr.colname: the column name for chromosome in data file.
pos.colname: the column name for chromosomal position in data file.
plot.dir: output DIR for graphs.
result.dir: output DIR for result txt files.


5)Run CMDS

5.1)To analyze all data files in the data DIR, please run
R --no-save < cmds.R 

5.2)To separately analyze data files 1,2,3,..., please run
R --no-save < cmds.R 1
R --no-save < cmds.R 2
R --no-save < cmds.R 3
...
(This allows you to parallelize jobs via a computer cluster, some cluster systems need a change of the code, see below)

Loctae the lines below in the function cmds.focal.test() in the file cmds_lib.R

png(out.image,1024,768)
#bitmap(out.image,width=1024,height=768,units="px")

Switch them like below

#png(out.image,1024,768)
bitmap(out.image,width=1024,height=768,units="px")


6)Output

Two result DIRs below will be created in your working DIR
cmds_test/ 
cmds_plot/ 

In cmds_tets/, each file contains 11 columns below

chromosome=>chromosome ID
window=>ordered window ID
start=>start position of a window
mid=>middle position of a window, 
end=>end position of a window
m=>CN mean of a window
m.sd=>SD of m
m.p=>p-value of test H0: m.sd=0, when m.p<cutoff (e.g. 0.001), m.sd>0 means amplification, m.sd<0 means deletion 
z=>the unstandardized RCNA score for the middle position of a window 
z.sd=>SD of z (i.e. the standardized RCNA score)
z.p=>p-value of test H0: z.sd=0, z.p<cutoff (e.g. 0.001) suggests a significant RCNA at the middle position in population

In cmds_plot/,  plots are produced for the columns m.sd, m.p, z.sd and z.p in the test files.

Reference Online:

Abstract: http://bioinformatics.oxfordjournals.org/cgi/content/abstract/btp708?ijkey=Z6cxCrjhtCwK2dE&keytype=ref

PDF: http://bioinformatics.oxfordjournals.org/cgi/reprint/btp708?ijkey=Z6cxCrjhtCwK2dE&keytype=ref

Contact: Qunyuan Zhang

E-mail: qunyuan@wustl.edu






