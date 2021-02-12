## Protocols
#### TruSeq Stranded mRNA (Poly A)
* input: 0.2-1ug total RNA in 50ul
* In LIMS: TruSeq Stranded mRNA
* Correct strand setting in pipeline: 2
* [TruSeq_stranded_mRNA_15031047E_October2013.pdf](../../assets/pdf/TruSeq_stranded_mRNA_15031047E_October2013.pdf)

#### TruSeq Stranded total RNA (ribominus)
* input: 0.1-1ug total RNA in 10ul
* In LIMS: TruSeq Strand totRNA
* Correct strand setting in pipeline: 2
* [TruSeq_stranded-totalRNA_15031048E_October2013.pdf](../../assets/pdf/TruSeq_stranded-totalRNA_15031048E_October2013.pdf)

#### SMART-Seq Stranded Kit (ribominus)
* input: 10pg-10ng in 7ul
* In LIMS: SMARTseq Strand RNA
* Correct strand setting in pipeline: 2
* Note: Currently no dual index
* [SMART-Seq_Stranded_Kit_User_Manual_070518.pdf](../../assets/pdf/SMART-Seq_Stranded_Kit_User_Manual_070518.pdf)

#### SMARTer® Stranded Total RNA-Seq Kit v2 - Pico Input Mammalian (ribominus)
* Used instead of SMART-Seq Strand RNA for FFPE samples
* In LIMS: SMARTer totRNA Pico
* Correct strand setting in pipeline: 2
* Note: Currently no dual index
* [UTF-8_27en-us_27SMARTer_Stranded_Total_RNA-Seq_Kit_v2_-_Pico_Input_Mammalian_User_Manual_063017.pdf](../../assets/pdf/UTF-8_27en-us_27SMARTer_Stranded_Total_RNA-Seq_Kit_v2_-_Pico_Input_Mammalian_User_Manual_063017.pdf)

#### SMART-Seq® v4 Ultra® Low Input RNA Kit for Sequencing (Poly A)
* input: 10pg-10ng in 9.5ul
* In LIMS: SMARTer RNA
* Correct strand setting in pipeline: 0
* [SMART-Seq_v4_Ultra_Low_Input_RNA_January2016.pdf](../../assets/pdf/SMART-Seq_v4_Ultra_Low_Input_RNA_January2016.pdf)

## Pipeline
Please see [RNA-seq pipeline](pipelines_scripts.md#RNA-seq)

## Follow-up analyses
### Non-topology based methods
Do not integrate existing knowledge regarding the positions and roles of the genes within the pathways, the directions and types of the signals transmitted from one gene to another, etc.

Recommended when: One needs to analyze arbitrarily defined sets of genes, rather than pathways

#### Tools
* **Best tool: GSEA** (Gene Set Enrichment Analysis):
    * [Enrichment_analysis_lecture_solution.pdf](../../assets/pdf/Enrichment_analysis_lecture_solution.pdf)
    * [GSEA software](http://software.broadinstitute.org/gsea/index.jsp)
    * [GSEA tutorial](../../assets/pdf/GSEA_howto.pdf)
* **Reactome:**
    * [https://reactome.org/](https://reactome.org/)
    * An open-source, open access, manually curated and peer-reviewed pathway database. The goal is to provide intuitive bioinformatics tools for the visualization, interpretation and analysis of pathway.
* **ReactomePA:**
    * R biocondutor package
    * GSEA analysis using reactome database
    * Organisms: celegans, fly, human, mouse, rat, yeast, zebrafish


### Topology-based methods
Provide a better ability to identify pathways that contain genes that caused the phenotype or are closely related to it

Recommended when:

* it is important to consider how various genes interact
* one wishes to take advantage of the sizes and directions of measured expression changes
* one wishes to account for the type and direction of interactions on a pathway
* one intends to predict or explain downstream or pathway-level effects
* one is interested in understanding the underlying mechanisms

#### Tools
* **Best tool: ROntoTools**
    * R bioconductor package
    * [ROntoTools](https://www.bioconductor.org/packages/release/bioc/html/ROntoTools.html)
* **Best tool: IPathwayGuide**
    * identify significant pathways without all the noise found in other approaches
    * considers the size, role, and position of each gene on the pathway as it models high-throughput sequencing data. This advanced approach allows users to quickly prioritize targets and pathways, avoiding false positive and false negative results.
    * graphical user interface tool
    * uses same impact analysis approach as ROntoTools
    * Organisms: human, rat, mouse
    * price:
        * free 72 hour preview of reports
        * each report USD 287
        * subscription option are also available
* **Ingenuity Pathway Analysis (IPA):**
    * [ingenuity-pathway-analysis](https://www.qiagenbioinformatics.com/products/ingenuity-pathway-analysis/)
    * builds on the comprehensive, manually curated content of the Ingenuity Knowledge Base to help scientists understand the biological context of their expression analysis experiments. Powerful algorithms combined with content provide unique advanced analysis capabilities to help you identify the most significant pathways and discover potential novel regulatory networks and causal relationships associated with your experimental data.
    * wide range of analysis and visualization methods
    * graphical user interface tool
    * price: expensive license

*(Nguyen et al. Genome Biology, 2019)*
