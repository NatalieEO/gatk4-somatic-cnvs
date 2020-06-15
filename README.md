# somatic-cnvs

### Purpose :
Workflows for somatic copy number variant analysis.
Bugsby

### cnv_somatic_panel_workflow :
Builds a panel of normals (PON) for the cnv pair workflow.

#### Requirements/Expectations

Important: The normal_bams samples in the json can be used test the wdl, they are NOT to be used to create a panel of normals for sequence analysis. For instructions on creating a proper PON please refer to user the documents [Panel of Normals](https://software.broadinstitute.org/gatk/documentation/article?id=11053) and [Generate a CNV panel of normals with CreateReadCountPanelOfNormals](https://gatkforums.broadinstitute.org/dsde/discussion/11682#2) .

The reference used must be the same between PoN and case samples.

- ``CNVSomaticPanelWorkflow.gatk_docker`` -- GATK Docker image (e.g., ``broadinstitute/gatk:latest``).
- ``CNVSomaticPanelWorkflow.intervals`` -- Picard or GATK-style interval list.  For WGS, this should typically only include the autosomal chromosomes.
- ``CNVSomaticPanelWorkflow.normal_bais`` -- List of BAI files.  This list must correspond to `normal_bams`.  For example, `["Sample1.bai", "Sample2.bai"]`.
- ``CNVSomaticPanelWorkflow.normal_bams`` -- List of BAM files.  This list must correspond to `normal_bais`.  For example, `["Sample1.bam", "Sample2.bam"]`.
- ``CNVSomaticPanelWorkflow.pon_entity_id`` -- Name of the final PoN file.
- ``CNVSomaticPanelWorkflow.ref_fasta_dict`` -- Path to reference dict file.
- ``CNVSomaticPanelWorkflow.ref_fasta_fai`` -- Path to reference fasta fai file.
- ``CNVSomaticPanelWorkflow.ref_fasta`` -- Path to reference fasta file.

In additional, there are optional workflow-level and task-level parameters that may be set by advanced users; for example:

- ``CNVSomaticPanelWorkflow.do_explicit_gc_correction`` -- (optional) If true, perform explicit GC-bias correction when creating PoN and in subsequent denoising of case samples.  If false, rely on PCA-based denoising to correct for GC bias.
- ``CNVSomaticPanelWorkflow.PreprocessIntervals.bin_length`` -- Size of bins (in bp) for coverage collection.  *This must be the same value used for all case samples.*
- ``CNVSomaticPanelWorkflow.PreprocessIntervals.padding`` -- Amount of padding (in bp) to add to both sides of targets for WES coverage collection.  *This must be the same value used for all case samples.*

Further explanation of other task-level parameters may be found by invoking the ``--help`` documentation available in the gatk.jar for each tool.

#### Outputs 
- Read count PON in HD5 format
- Addtional metrics

### cnv_somatic_pair_workflow :
Running a matched pair to obtain somatic copy number variants.

#### Requirements/Expectations

The reference and bins (if specified) must be the same between PoN and case samples.

- ``CNVSomaticPairWorkflow.common_sites`` -- Picard or GATK-style interval list of common sites to use for collecting allelic counts.
- ``CNVSomaticPairWorkflow.gatk_docker`` -- GATK Docker image (e.g., ``broadinstitute/gatk:latest``).
- ``CNVSomaticPairWorkflow.intervals`` -- Picard or GATK-style interval list.  For WGS, this should typically only include the autosomal chromosomes.
- ``CNVSomaticPairWorkflow.normal_bam`` -- Path to normal BAM file.
- ``CNVSomaticPairWorkflow.normal_bam_idx`` -- Path to normal BAM file index.
- ``CNVSomaticPairWorkflow.read_count_pon`` -- Path to read-count PoN created by the panel workflow. 
- ``CNVSomaticPairWorkflow.ref_fasta_dict`` -- Path to reference dict file.
- ``CNVSomaticPairWorkflow.ref_fasta_fai`` -- Path to reference fasta fai file.
- ``CNVSomaticPairWorkflow.ref_fasta`` -- Path to reference fasta file.
- ``CNVSomaticPairWorkflow.tumor_bam`` -- Path to tumor BAM file.
- ``CNVSomaticPairWorkflow.tumor_bam_idx`` -- Path to tumor BAM file index.

In additional, there are several task-level parameters that may be set by advanced users as above.

To invoke Oncotator on the called tumor copy-ratio segments:

- ``CNVSomaticPairWorkflow.is_run_oncotator`` -- (optional) If true, run Oncotator on the called copy-ratio segments.  This will generate both a simple TSV and a gene list.


Further explanation of these task-level parameters may be found by invoking the ``--help`` documentation available in the gatk.jar for each tool.

#### Outputs
- Modeled segments for tumor and normal
- Modeled segments plot for tumor and normal
- Denoised copy ratios for tumor and normal
- Denoised copy ratios plot for tumor and normal
- Denoised copy ratios lim 4 plot for tumor and normal
- Addtional metrics 

### Software version requirements :
- GATK4.1.1.0  

Cromwell version support 
- Successfully tested on v37

### Important Note :
- Runtime parameters are optimized for Broad's Google Cloud Platform implementation.
- For help running workflows on the Google Cloud Platform or locally please
view the following tutorial [(How to) Execute Workflows from the gatk-workflows Git Organization](https://software.broadinstitute.org/gatk/documentation/article?id=12521).
- The following material is provided by the GATK Team. Please post any questions or concerns to one of our forum sites : [GATK](https://gatkforums.broadinstitute.org/gatk/categories/ask-the-team/) , [FireCloud](https://gatkforums.broadinstitute.org/firecloud/categories/ask-the-firecloud-team) or [Terra](https://broadinstitute.zendesk.com/hc/en-us/community/topics/360000500432-General-Discussion) , [WDL/Cromwell](https://gatkforums.broadinstitute.org/wdl/categories/ask-the-wdl-team).
- Please visit the [User Guide](https://software.broadinstitute.org/gatk/documentation/) site for further documentation on our workflows and tools.

### LICENSING :
Copyright Broad Institute, 2019 | BSD-3
This script is released under the WDL open source code license (BSD-3) (full license text at https://github.com/openwdl/wdl/blob/master/LICENSE). Note however that the programs it calls may be subject to different licenses. Users are responsible for checking that they are authorized to run all programs before running this script.
