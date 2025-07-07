<div class="sticky-glossary">

**Glossary** \
- 

</div>

Usually, after the Histopathology report, the gastroenterologist determines if DNA/RNA sequencing is required. 

#### Sectioning for molecular analysis
If sequencing is needed, the FFPE block is retrieved and the area of interest is sectioned. This section is sent to a molecular path lab. 

#### DNA/RNA extraction
The section is sent to a molecular path lab where DNA and/or RNA is extracted. 

??? info "For those interested in additional details click here, else skip to [Library Preparation and Sequencing](#library-preparation-and-sequencing)" 
    Here the chemical processes remove paraffin and extract DNA and/or RNA. If multiple extractions are performed from the same block (e.g., DNA and RNA separately, or from different regions of the block), each extract will get its own unique identifier. The tiny tubes or wells in a multi-well plate (e.g., a 96-well plate) containing the extracted DNA or RNA are immediately labeled with the new Molecular Lab ID, almost always in barcode format. 
    
    Molecular Lab assigns a new, unique internal Molecular Lab ID to the extracted DNA/RNA sample. This new Molecular Lab ID is cross-referenced and linked in the EMR to the original pathology accession number of the FFPE block, which in turn is linked to the patient's medical record number and demographics. Other data captured include the date and time of extraction, the specific extraction kit/protocol used, QC metrics for the extracted nucleic acids (e.g., concentration, purity ratios like A260/280, DNA integrity metrics, RNA integrity number (RIN) or DV200 for FFPE RNA), the physical location where the extracted nucleic acid sample is stored (e.g., freezer box, shelf, rack).

    Quality and quantity of the extract is important for the sequencing. Extracted samples are subjected to quality control for purity, concentration and suitability for sequencing. QC generates small reports on DNA/RNA concentration and purity.

#### Library Preparation and Sequencing
DNA/RNA is fragmented and adapters added. The sequencing facility's LIMS will integrate this Molecular Lab ID (often incorporated into the file naming conventions for the resulting FASTQ files) and follow the sample. The library is sequenced on a platform like Illumina, PacBio, or Oxford Nanopore. This generates raw data in a proprietary format (e.g., BCL, FAST5).

??? info "For those interested in additional details click here, else skip forward to [Base Calling](#base-calling-output---fastq-file)"
    DNA/RNA libraries are loaded into the sequencer, which reads the order of the nucleotide bases (A,T,C,G for DNA or A,U,C,G for RNA) for the fragment of interest. The raw sequencing data is processed computationally - (a) compared with reference human genome, (b) identified variants (c) quantified gene expression for RNA sequencing. This is the first step when the physical DNA/RNA is converted to digital data. 

    The sequencing platform generates an output in proprietary format as it reads the DNA/RNA. e.g., Illumina's sequencers produce BCL files, PacBio produces BAM files with specific reads, Oxford Nanopore produces FAST5 files. It's platform-specific and usually not directly interpretable by end-users without specialized software.

##### Base Calling Output - FASTQ file
Base calling software converts signals into nucleotide sequences and quality scores. This is via a process called base calling. The output of base calling is the FASTQ file. The FASTQ file is the universal format for raw reads. This data is considered the primary raw sequence data for downstream bioinformatics analysis.

??? info "For those interested in additional details … read here, else skip forward to [SAM/BAM Files](#alignment-or-mapping---sambam-files)"
    The crucial link between the raw FASTQ files and the patient/specimen metadata happens outside the FASTQ file itself, through robust laboratory information management systems (LIMS) and file naming conventions. The most common and direct way to link FASTQ files to a sample is through standardized file names. Labs typically adopt a strict naming convention that includes a unique sample ID, which in turn can be traced back to the patient. Example: PatientID_SampleID_Lane_ReadPair_Index.fastq.gz or similar. 
    
    This SampleID is the key that links the computational data back to the physical sample. Sample ID is essentially the Molecular Lab ID. Sample Id is often used somewhat interchangeably with the "Molecular Lab ID" or a derivative of it. When the samples are prepared for a sequencing run (library preparation), this Sample ID is the primary identifier used to label the libraries and, consequently, the resulting FASTQ files.

##### Alignment or Mapping - SAM/BAM Files
Bioinformatics tools (e.g., BWA, Bowtie2, HISAT2 for RNA-seq) take these raw sequence reads and computationally align them (map them) to a known reference genome. SAM is a text format, while BAM is the binary, compressed version used for efficient analysis.

- SAM (Sequence Alignment/Map) is the human-readable, text-based format of the alignment results. 
- BAM (Binary Alignment/Map): This is the compressed, binary version of SAM. BAM files are significantly smaller, indexed (allowing for quick access to specific regions), and much more efficient for storage and processing by downstream bioinformatics tools. For most practical purposes, BAM files are preferred over SAM.

??? info "For those interested in additional details click here, else skip forward to [VCF](#variant-calling---vcf-variant-call-format-file)"
The most immediate way to identify a SAM/BAM file is through its filename. Standard naming conventions include the Sample ID (which, as discussed, usually corresponds to the Molecular Lab ID). The header section of a SAM/BAM file (which starts with @ lines) contains crucial information that includes Sample/Molecular Lab Id. A single BAM file might theoretically contain reads from multiple samples. More often, a BAM file is specific to one sample, and the @RG header simply confirms its identity. @RG is the primary mechanism for linking reads within a BAM file to a specific sample, library, or sequencing run. 

##### Variant Calling - VCF (Variant Call Format) File
After the reads are successfully aligned to the reference genome (in BAM format), the next critical step is variant calling. Variant calling software (e.g., GATK HaplotypeCaller, VarScan2, FreeBayes) analyzes the aligned reads in the BAM files to identify genetic differences (variants) between the sequenced sample and the reference genome. VCF is a standardized text file format that stores information about genomic variants at specific chromosomal positions.

??? info "For those interested in additional details click here, else skip forward to [Clinical Interpretation and Molecular Path Report](#clinical-interpretation-and-molecular-pathology-report)"
    Like FASTQ and BAM files, VCF files are typically named with the Sample ID (Molecular Lab ID) to ensure quick identification. VCF files have extensive header lines starting with ## (meta-information lines) and a single header line starting with # for column names. This is where sample information is explicitly stated. Sample Columns in the Header Line is the most direct and crucial way VCF files identify samples. If a VCF file contains genotype information for multiple samples, the sample names (IDs) are listed as columns after the FORMAT column in the header line. This allows each row of variant data to then have genotype information for each listed sample in the subsequent columns. Each data line in a VCF file represents a variant. Following the FORMAT column, there are columns for each sample, labeled with their respective Sample IDs from the header line. These columns contain the genotype, allelic depth, genotype quality, and other relevant information for that specific sample at that variant position.

The BAM and VCF files are typically the primary inputs for the subsequent clinical interpretation.

#### Clinical Interpretation and Molecular Pathology Report
A molecular pathologist reviews the variants in context with the patient’s clinical findings. A detailed report is generated, referencing the BAM and VCF files and other metadata like assay, project, and sample identifiers.

A molecular path report metadata is typically captured digitally. That would include the project, assay, sample identifier, patient identifier, location of the raw file, the .bam/.sam and the vcf files.

[← Previous](at-histopath.md) | [Next →](end-note.md)