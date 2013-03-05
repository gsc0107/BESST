BESST
======
 
INSTALLATION:
--------------
In case you don't have the installer "pip" installed, type (in terminal):

easy_install pip

Then do:

sudo pip install BESST

Now you should be able to run:

runBESST -h

to view user instructions.

(This installation will install the two nonstandard python module dependencies: pysam 0.6, networkx-1.6 automatically for you.)

INPUT:
-------
Requierd arguments:

* -c <a contig file> (path to) 

*  -f <the bamfiles>  (paths to)

* -o <path to the location to where you want the output>

EXAMPLE RUN:

runBESST -c /path/to/contigfile.fa -f /path/to/file1.bam /path/to/file2.bam -o /path/to/output 

Optional arguments:

The following arguments are computed internally by BESST. It is however good to specify mean and standard deviation if your assembly is very fragmented compared to the library insert size (not enough large contains to compute library statistics on).

* -m  <the means of the insert sizes of the library, one integer number for each library> (integer numbers)

* -s <standard deviation of the libraries, one integer number for each library> (integer numbers)

* -T <Thresholds that are some upper levels that you think not too many PE/MP will have a longer insert size than (in the end of the mode)> (integer numbers) 

7 -r <Mean read length for each of the libraries> (integer number) 

8 -e <The least amount of witness links that is needed to create a link edge in graph (one for each library)> (integer number) 

10 -k  <Minimum contig size to include in the scaffolding in each scaffolding step>  (One number for each library (default 0) (integer numbers))

12 -z <Coverage cutoff for repeat classification> [ e.g. -z 100 says that contigs with coverages over 100 will be discarded from scaffolding.] (integer numbers, one for each library) 

13 -g  <Haplotype detection function on or off> [ default = 0 (off) <0 or 1>]

14 -a  <Maximum length difference ratio for merging of haplotypic regions> (float nr)

15 -b <Nr of standard deviations over mean/2 of coverage to allow for clasification of haplotype> (integer value) 

16 -d <check for sequencing duplicates and count only one of them (when computing nr of links) if they are occurring> (default on = 1). <0 or 1> 

17 -y < 0 or 1 > Extend scaffolds with smaller contains (default on).


NOTE:

1 Definition of insert size: BESST assumes the following definitions: insert size = fragment length. 
For PE: 
   s                    t
   ------>      <-------
from s to t, that is, insertsize = readlen1 + gap + readlen2. So when mean and/or threshold is supplied, it should be mean and threshold of this distance.

2 Mapping reads: If you want to map a mate pair library, you will need to map them as paired end library i.e. forward-revers mode. All read pair libraries should be in this order.

3 Order of scaffolding: It is crucial for the algorithm that you give the libraries in increasing order of the insert size.


VERSIONS

Some versions are only included for history purposes (within () ).

(V0.5
 * Added parameter prefix -o <path> that gives the user ability to choose his own output directory
 * Added required parameter -r that is integers denoting the mean read length
 * Added optional parameter -s that lets you specify the standard deviation of the library (one for each library). If specified, the gap size is calculated with a sophisticated formula. Else, it's calculated naively.
 * Added output of a gff version 3 file
 * Added so -t parameter chan take different values for diff. libraries)
 

V0.6
 BUG FIXES
 * fixed bug in -e parameter, before some special cases when e < parameter_value could still be included in graph and final scaffolding step. Now, no such edge will be included.
 * fixed bug in positions of contains within scaffolds 
 * Fixed +1bp index bug in GFF file output 
 OTHER STUFF
 * Implemented gap estimation tool. With a narrowed down interval to gap in [-2*std_dev, mean+2*std_dev]
 * Fixed so that negative gaps are set to 0 in gap estimate.
 * Fixed so that scaffolds are written out properly in scaffolds.fa (no abutting contigs when we have negative gaps).
 * Outputs number of links between contigs to the GFF file (that gave rise to the edge in the graph).
 * -e parameter is now set independently set for each library e.g. -e 5 5 10 5 3
 * non-naive gap estimation is only active if nr of links is  >=  5, independent of what -e is set to.
 * Gap fragments added to GFF file
 * Scaffolds are printed out with 60 characters per line
 * More information is printed to stdout: all parameters specified for each library

V0.7

* Fixed various minor bugs.
* Changed way to create edges from BAM file, more paired-reads are now used.
* Removed some reads that are not unique by looking at tags specified by BWA (mapping quality > 10)
* Implemented coverage attribute for each contig.
* Implemented repeat removal based on the coverage information for a contig.
* Restructured code when passing function arguments between functions. Now, we have a new object caller Parameter that stores this to make the code more readable.
* Haplotype detector
* Removed required parameter -m (made optional). If not specified, it is calculated from the BAM file.
* Removed required parameter -s (made optional). If not specified, it is calculated from the BAM file.
* Removed required parameter -T (made optional). Set by program default -T = mean + 4*std_dev 
* Removed required parameter -t.
* Added optional parameter -k. Consider contigs larger than -k at each step. Default set to: mean + (std_dev/mean)*std_dev
* Added optional parameter -g.  Detect and treat haplotypes, default value of g is 1 , i.e. function is on. To switch off, set -g 0
* Added optional parameter -d Detect and remove duplicates,  default value of g is 1 , i.e. removing duplicates. To switch off, set -g 0. Removeing dupl. based on identical mapping positions for both reads in pair (this will give some false positives)
* Removed matplotlib dependency
* Removed scipy,numpy dependency by using closed formula approximation for erf(x)
* Print out scaffolds created for each pass in the algorithm

V1.0
* Various bug fixes
* Implemented scaffolding with smaller contigs by searching for coherent paths
* New score on edges in scaffold graph
* a beta version of parallelization of the small contains connection finder. specified by option -q 1 (default 0).



