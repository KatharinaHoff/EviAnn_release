# EviAnn -- evidence-based eukaryotic genome annotation pipeline

EviAnn (Evidence Annotation) is a novel annotation pipeline.  EviAnn does not use any de novo gene finders in its processing.  It is purely evidence-based.  EviAnn uses RNAseq data and/or transcripts and proteins from related species as inputs, produces annotation of protein coding genes and transcripts, and outputs it in GFF3 format.  EviAnn does not require genome repeats to be soft-masked prior to running annotation.  EviAnn is stable and fast. Annotation of A.thaliana genome takes about 2 hours on a single 32-64 core server (not including time for aligning RNAseq reads, which could vary depending on the amount of data used.) 

# Installation insructions

To install, first download the latest distribution tarball EviAnn-X.X.X.tar.gz (not one of the Source files) from the github release page https://github.com/alekseyzimin/EviAnn_release/releases. Replace X's below with the version number. Then run:
```
$ tar xvzf EviAnn-X.X.X.tar.gz
$ cd EviAnn-X.X.X
$ ./install.sh
```
The installation script will configure and make all necessary packages.  The EviAnn executables will appear under bin/.  You can run EviAnn from anywhere by executing /path_to/EviAnn-X.X.X/bin/eviann.sh

## Dependencies:

EviAnn requires the following external dependencies to be installed and available on the $PATH:

1. minimap2: https://github.com/lh3/minimap2
2. HISAT2: https://github.com/DaehwanKimLab/hisat2

Here is the list of the dependencies included with the package:

1. stringtie version 2.2.1 -- static executable
2. gffread version 0.12.7 -- static executable
3. gffread version 0.12.6 -- static executable
4. blastp version 2.13.0+ -- static executable
5. tblastn version 2.13.0 -- static executable
6. makeblastdb version 2.13.0 -- static executable
7. exonerate version 2.4.0 -- static executable
8. TransDecoder version 5.7.1
9. samtools version 0.1.20
10. ufasta version 1

## Only for developers

You can clone the development tree, but then there are dependencies such as swig and yaggo (http://www.swig.org/ and https://github.com/gmarcais/yaggo) that must be available on the PATH:

```
$ git clone https://github.com/alekseyzimin/EviAnn_release
$ cd EviAnn_release
$ git submodule init
$ git submodule update
$ cd ../ufasta && git checkout master
$ cd ..
$ make
$ (cd build/inst/bin && tar xzf TransDecoder-v5.7.1.tar.gz)
```
To create a distribution, run 'make install'. Run 'make' to compile the package. The binaries will appear under build/inst/bin.  
Note that on some systems you may encounter a build error due to lack of xlocale.h file, because it was removed in glibc 2.26.  xlocale.h is used in Perl extension modules used by EviAnn.  To fix/work around this error, you can upgrade the Perl extensions, or create a symlink for xlocale.h to /etc/local.h or /usr/include/locale.h, e.g.:
```
ln -s /usr/include/locale.h /usr/include/xlocale.h
```

# Usage:
```
$ ./build/inst/bin/eviann.sh -h
Usage: eviann.sh [options]
Options:
-t <number of threads, default:1>
-g <MANDATORY:genome fasta file with full path>
-p <file containing list of filenames of paired Illumina reads from RNAseq experiments, one pair of /path/filename per line; fastq is expected by default, if files are fasta, add "fasta" as the third field on the line>
-u <file containing list of filenames of unpaired Illumina reads from RNAseq experiments, one /path/filename per line; fastq is expected by default, if files are fasta, add "fasta" as the third field on the line>
-e <fasta file with transcripts from related species>
-r <fasta file of protein sequences from related species>
-m <max intron size, default: 100000>
--debug <debug flag, if used intermediate output files will be kept>
-v <verbose flag>

-r AND one or more of the -p -u or -e must be supplied.
```


