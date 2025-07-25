
<!---
The page title should not go in the menu
-->
<p class="page-title">File Formats: Genomes</p>

## chrom.sizes

igvtools uses chrom.sizes files to define the chromosome lengths for a given genome.    The file format is tab delimited, first column is chromosome name and second is its length.  There can be more columns present, but they are ignored.  Files should be named as follows:

```<genomdID>.chrom.sizes```

For example,  hg18.chrom.sizes.

## Cytoband

The Cytoband file format is used to 
  
* define the chromosome ideograms for a reference genome, or 
* create a cytoband track (as of version 2.11.0).

A cytoband file is a five-column tab-delimited text file. Each row of the file describes the position of a cytogenetic band. The columns in the file match the columns of the [cytoBand table](http://genome.ucsc.edu/cgi-bin/hgTables?db=hg38&hgta_group=map&hgta_track=cytoBand&hgta_table=cytoBand&hgta_doSchema=describe+table+schema) in the UCSC Genome Browser database.  These files are downloadable from the UCSC website as "cytoBandIdeo.txt.gz" for many genome assemblies, for example [https://hgdownload.cse.ucsc.edu/goldenPath/hg38/database/cytoBandIdeo.txt.gz](https://hgdownload.cse.ucsc.edu/goldenPath/hg38/database/cytoBandIdeo.txt.gz)

<table class="general" width="100%">
	<tbody>
		<tr>
			<th>
				Column</th>
			<th>
				Example</th>
			<th>
				Data Type</th>
			<th>
				Description</th>
		</tr>
		<tr>
			<td>
				chrom</td>
			<td>
				chr1</td>
			<td>
				string</td>
			<td>
				Chromosome</td>
		</tr>
		<tr>
			<td>
				chromStart</td>
			<td>
				0</td>
			<td>
				integer</td>
			<td>
				<p>Start position in chromosome sequence</p>
			</td>
		</tr>
		<tr>
			<td>
				chromEnd</td>
			<td>
				2300000</td>
			<td>
				integer</td>
			<td>
				End position in chromosome sequence</td>
		</tr>
		<tr>
			<td>
				name</td>
			<td>
				p36.33</td>
			<td>
				string</td>
			<td>
				Name of cytogenetic band</td>
		</tr>
		<tr>
			<td>
				gieStain</td>
			<td>
				gneg</td>
			<td>
				string</td>
			<td>
				Giemsa stain results. Recognized stain values: gneg, gpos50, gpos75, gpos25, gpos100, acen, gvar, stalk</td>
		</tr>
	</tbody>
</table>

## FASTA

The [FASTA](https://www.ncbi.nlm.nih.gov/genbank/fastaformat/) file format (.fasta or .fa) is used to specify the reference sequence for an imported genome. Each sequence in the FASTA file represents the sequence for a chromosome. The sequence name in the FASTA file is the chromosome name that appears in the chromosome drop-down list in the IGV tool bar. 

IGV requires that the FASTA file be indexed.  The index file can be created with [samtools](https://www.htslib.org/) or igvtools.  The index file is named the same as the FASTA file, but with a ".fai" extension.  For example, if the FASTA file is named "hg18.fasta", the index file should be named "hg18.fasta.fai".  If the index file is not present IGV will attempt to create one.  

FASTA files can be loaded directly from the **_Genome_** menu or can be referred to in a [JSON](#igv-reference-genome-json) file that contains a reference genome specification.  

## TWOBIT

The [2bit](https://genome.ucsc.edu/FAQ/FAQformat.html#format2bit) file format is a compressed binary format for storing DNA sequence data.  It can be used in lieu of a FASTA file for defining the reference.  The 2bit file can be created from a FASTA file using the [faToTwoBit](https://genome.ucsc.edu/goldenPath/help/twoBit.html) program from UCSC.

## IGV reference genome (JSON)

As of release 2.11.0 reference genomes can be specified and loaded as JSON files.  The previous ".genome" format is now considered deprecated.  The format is a json form of the "reference" object description from igv.js, described [here](https://github.com/igvteam/igv.js/wiki/Reference-Genome).  For IGV use, required properties include ```id```, ```name```, and either ```fastaURL``` and ```indexURL``` or ```twoBitURL```.  All other properties are optional.  An example of a complete json description for the GRCh38 assembly is given below.

Fields ending with "URL" can contain local file paths.  These paths can be absolute or relative to the location of the genome (.json) file.

**Example: Human GRCh38 with 2 annotation tracks**

Required fields are ```id```,```name```, ```fastaURL```, ```indexURL```.   All other fields are optional.

```json
{
  "id": "hg38",
  "name": "Human (GRCh38/hg38)",
  "fastaURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg38/hg38.fa",
  "indexURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg38/hg38.fa.fai",
  "cytobandURL": "https://s3.amazonaws.com/igv.org.genomes/hg38/annotations/cytoBandIdeo.txt.gz",
  "aliasURL": "https://s3.amazonaws.com/igv.org.genomes/hg38/hg38_alias.tab",
  "chromosomeOrder": [
    "chr1",
    "chr2",
    "chr3",
    "chr4",
    "chr5",
    "chr6",
    "chr7",
    "chr8",
    "chr9",
    "chr10",
    "chr11",
    "chr12",
    "chr13",
    "chr14",
    "chr15",
    "chr16",
    "chr17",
    "chr18",
    "chr19",
    "chr20",
    "chr21",
    "chr22",
    "chrX",
    "chrY"
  ],
  "tracks": [
    {
      "name": "Refseq Genes",
      "format": "refgene",
      "url": "https://s3.amazonaws.com/igv.org.genomes/hg38/ncbiRefSeq.sorted.txt.gz",
      "indexURL": "https://s3.amazonaws.com/igv.org.genomes/hg38/ncbiRefSeq.sorted.txt.gz.tbi"
    },
    {
      "name": "Gencode v24 genes",
      "format": "gtf",
      "url": "https://s3.amazonaws.com/igv.org.genomes/hg19/gencode.v24.genes.gtf.gz"
    }
  ]
}

```

**File paths**

URL properties (all fields that end with ```url```) can be absolute or relative file paths.  Relative paths are interpreted as relative to the location of the genome json file.   For example, the following definition presumes an annotation file ```chr22.genes.gtf.gz``` in the same directory as the json file.

hg19_local_annotations.json
```
{
  "id": "hg19",
  "name": "Human (CRCh37/hg19)",
  "fastaURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg19/hg19.fasta",
  "indexURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg19/hg19.fasta.fai",
  "cytobandURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg19/cytoBand.txt",
  "aliasURL": "https://s3.amazonaws.com/igv.org.genomes/hg19/hg19_alias.tab",
  "tracks": [
    {
      "name": "Gencode v24 genes",
      "url": "chr22.genes.gtf.gz"
    },

  ]
}
```


**Genome with hidden annotation track**

In the example below an annotation file containing protein coding genes from Gencode is loaded to support searching by Gencode gene identifiers.  


```json
{
  "id": "hg19",
  "name": "Human (CRCh37/hg19)",
  "fastaURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg19/hg19.fasta",
  "indexURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg19/hg19.fasta.fai",
  "cytobandURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg19/cytoBand.txt",
  "aliasURL": "https://s3.amazonaws.com/igv.org.genomes/hg19/hg19_alias.tab",
  "tracks": [
    {
      "name": "Refseq Genes",
      "url": "https://hgdownload.soe.ucsc.edu/goldenPath/hg19/database/ncbiRefSeq.txt.gz"
    },
    {
      "url": "https://s3.amazonaws.com/igv.org.genomes/hg19/gencode.v24.genes.gtf.gz",
      "hidden": true
    }
  ]
}
```
