#cTools

<!--This is ~np29/biology/cteam/progs.info
Executables in ~np29/bin
List of accessible samples: /home/np29/cteam/release/cteam.ind
This is Reich lab only.  Need to make smaller file for public
-->
This document is the instruction of using the cTools to access the light version (compressed) Simon's genomic diversity project (SGDP lite) files. The cTools include:
1. cuncompress
2. cascertain
3. cpulldown
4. cpoly
<!-- 
Two programs (cascertain, cpulldown) to access C team
This is an alpha release and some options surely need to be added.
-->

SGDP lite includes the full C team data and the following high quality ancient genomes (extended C-team):
<!--I use a database I call the "extended C-team" which in addition to the full C team
has high quality ancient genomes:
-->
               Altai  F            Altai
            Denisova  F        Denisovan
           Loschbour  M              WHG
           Stuttgart  F              LBK
           Ust_Ishim  M        Ust_Ishim

Currently, there are 341 samples in SGDP lite: <br />

- FullyPublicGeneral: 164 samples (including the above ancient genomes) <br />
- FullyPublicHGDP: 120 samples <br />
- SignedLetterGeneral: 9 samples <br />
- SignedLetterDiRienzo: 4 samples <br />
- SignedLetterTishkoff: 44 samples
<!--the above are "extended C-team files";  max filter value 1.  Reich lab only (!)
These can be accessed by the software just as though they are part of C-team
-->
##How to download the SGDP lite sample files?
##Configuration (Only needed when you customized the architecture of your downloaded file folders.)
##How to access the data using cTools?
To access the sample files using cascertain, cpulldown and cpoly, these downloaded sample files (\*.ccomp.fa.gz and *.ccompmask.fa.gz) need to be uncompressed using cuncompress first. Moreover, Samtools is needed (accessable by command "Samtools") to run cuncompress properly. 

1. cuncompress -i sgdpsampname [-w workdir] <br />
cuncompress uncompress the \*.ccomp.fa.gz and *.ccompmask.fa.gz files. <br />
<br/>
   Inputs: \*.ccomp.fa.gz, \*.ccompmask.fa.gz <br />
Outputs: <br />

   Notes: The compressed input files are required in the workdir to allow cuncompress work properly. The output files are in the workdir. Samtools is required by cuncompress.

2. cascertain -p parfile [options] <br />
	options: <br />
	-V	verbose <br />
	-v	Show version information. <br />
	-? 	Show the instruction.

   Inputs: criteria to ascertain SNP.<br/>
   Outputs: .snp file (Reich lab format)

   Sample parfile: <br />

   ```
   snpname: ssout.snp 
   ascertain: S_Yoruba-1::1:2,Altai::1:1;S_Yoruba-1::1:2,Denisova::1:1 
   noascertain: Altai::1:2,Denisova::1:2
   ```
   In the above sample parfile, <br />  
   snpname: gives name to the output .snp file.

   ascertain:
There is a mini-language for ascertainment.  Allele 1 = derived, 0 = ancestral (where Chimp allele is ancestral)
In the ascertainment string we have substrings separated by ';'  each is an ascertainment rule.
Then each rule has substrings separated by ',' ;
Each substring is of form Sample_ID::a:b  This means we require sample to have a derived allele(s) out of b. a is the number of derived alleles; b is the total number of alleles in the sample. 

   noascertain: has the same syntax.   If this "hits" then the SNP is rejected.
Thus the example above means
ascertain if S_Yoruba_1 is a het and either Altai or Denisova has a derived allele (chosen at random)
 EXCEPT don't ascertain if both Altai and Denisova are hets.

   A full parameter list of the parfile:

   
 <!--  
   regname:	chromosome name [default: all the chromosomes]
   snpname:	the output snp file name (.snp)
   pagesize:
   minfilterval:	the base quality threshold for taking the genotype information; The 					quality values are in the mask file. C team has base quality in range 					(0-9) or no value (N/?) => don't use. Select bases with minfilterval: 3 					(say) This selects bases with base quality >=3. [1 is default and 					recommended for most applications]. Note that the extended C-team files 					such as Altai have manifesto filters (made in Leipzig) that are just 0, 1.  					If you are using extended C-team do not set minfilterval.
   seed:
   ascertain:	ascertain criteria
   noascertain: rejection criteria
   dbhetfa:	table of hetfa files (.dblist); [default: /home/mz128/cteam/dblist/			hetfa_postmigration.dblist] 
   dbmask: table of mask files (.dblist); [default: /home/mz128/cteam/dblist/			mask_postmigration.dblis]
   transitions:	work on transitions; [default: Yes]
   transversions:	work on transversions; [default: Yes]
   abxmode:
   minchrom:
   maxchrom:
   chrom:
   monosamples:
   monoval:
   lopos:	beginning coordinate of the region	[default: the beginning of the chromosome]
   hipos:	endding coordinate of the region [default: the end of the chromosome]
   -->
   
   | parameter     | description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| regname       | chromosome name [default: all the chromosomes]                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| lopos         | the beginning coordinate of the region[default: the beginning of the chromosome]                                                                                                                                                                                                                                                                                                                                                                                                                    |
| hipos         | the ending coordinate of the region [default: the end of the chromosome]                                                                                                                                                                                                                                                                                                                                                                                                                            |
| snpname       | the output snp file name (.snp)                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| minfilterval  | the base quality threshold for taking the genotype information; The,quality values are in the mask file. C team has base quality in range,(0-9) or no value (N/?) => don't use. Select bases with minfilterval: 3 (say). This selects bases with base quality >=3. [1 is default and,recommended for most applications]. Note that the extended C-team files,such as Altai have manifesto filters (made in Leipzig) that are just 0, 1. If you are using extended C-team do not set minfilterval. |
| ascertain     | ascertain criteria                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| noascertain   | rejection criteria                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| dbhetfa       | table of hetfa files (.dblist); [default: /home/mz128/cteam/dblist/,hetfa_postmigration.dblist]                                                                                                                                                                                                                                                                                                                                                                                                  |
| dbmask        | table of mask files (.dblist); [default: /home/mz128/cteam/dblist/,mask_postmigration.dblis]                                                                                                                                                                                                                                                                                                                                                                                                     |
| transitions   | work on transitions; [default: Yes]                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| transversions | work on transversions; [default: Yes]
|  minchrom:	| the beginning chromosome
|  maxchrom:	| the ending chromosome
   
   Notes: You can write comments in the parameter file by `#comments`.

3. cpulldown  -p parfile [options] <br />

   options: <br />
	-V	verbose <br />
	-c	checkmode <br />
	-v	Show version information. <br />
	-? 	Show the instruction. 
   
 
   Inputs: .snp file, .ind file (Reich lab format)

   Sample parfile
   
   ```
   D1:          /home/np29/biology/cteam/mixdir
   D2:          D1
   S2:           sc
   indivname:    D1/mix1.ind
   snpname:      D1/s01:01.snp
   indivoutname:     D2/S2.ind
   snpoutname:       D2/S2.snp
   genotypeoutname:  D2/S2.geno
   outputformat:     eigenstrat
   maxchrom:         22
   ```
   This is very similar to the parameter file for convertf which most of you will have used.
 indivname should be a .ind file (Reich lab format). Any .snp file is OK here, it needn't be output from cascertain.

   <!--
   See /home/np29/biology/cteam/info/cteamsmall.ind for the current extended C-team list.
   Only 214 samples at present owing to a system crash.  This will be increased to > 290 very     shortly.
   For now any samples you access should be a subset of cteamsmall.ind.
   -->

   A full parameter list of the parfile:
   
   <!--   minfilterval: similar to the minfilterval of  cascertain but
   *** default is 0 ***;  If you know a marker is truly polymorphic less filtering is      justified.
	genotypename:
	snpname:	input .snp file
	indivname:	input .ind file	
	indivoutname:	output .ind file
	snpoutname:	output .snp file
	genotypeoutname:	output .geno file
	outputformat:	output format
	minchrom:	
	maxchrom:
	chrom:
	-->
	
   | parameter       | description        |
   |-----------------|--------------------|
   | snpname         | input .snp file    |
   | indivname       | input .ind file    |
| indivoutname    | output .ind file   |
| snpoutname      | output .snp file   |
| genotypeoutname | output .geno file  |
| outputformat    | output format      |
| minfilterval    | similar to the minfilterval of  cascertain, but [default is 0] |
|  minchrom:	| the beginning chromosome
|  maxchrom:	| the ending chromosome



   Notes: Running time: Linear in number of samples in indivname.  10 samples pull down
in about 1 hour on orchestra. <!--so a pull down of the whole C-team is about 24 hours.
-->
4. cpoly
DOCUMENTATION NEEDED
<!--
4) ccompress -i sgdpsampname [-w workdir]
Makes files workdir/sgdpampname.comp.fa.gz and workdir/ssgdpname/compmask.fa.gz
-->
##Related file formats

<!--
Written by Nick on 6/15/14
Revised by Mengyao Zhao
Last revision: 11/26/14
-->