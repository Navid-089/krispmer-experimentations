# 🧬 Krispmer Experimentation Pipeline - Progress

## Datasets Used

| Name | Link |
| :--- | :--- |
| **Genome Assembly Rice\_IR8\_v1.7** | [https://www.ncbi.nlm.nih.gov/datasets/genome/GCA\_001889745.1/](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_001889745.1/) |
| **Genome Assembly Rice\_N22** | [https://plants.ensembl.org/Oryza\_sativa\_n22/Info/Index](https://plants.ensembl.org/Oryza_sativa_n22/Info/Index) |
| **Genome Assembly Basmati** | Stored before in the VM|
| **Genome Assembly Rice\_Japonica (annotated)** | [https://www.ncbi.nlm.nih.gov/datasets/genome/GCF\_034140825.1/](https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_034140825.1/) |
| **Genome assembly R498.Genome.version1 (Indica reference + annotated)** | [https://www.ncbi.nlm.nih.gov/datasets/genome/GCA\_002151415.1/](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_002151415.1/) |

-----

## Reference Genome Choice

All compared genomes (Basmati, IR8, N22) are from **Indica rice**, so we selected the **Indica R498 reference genome (GCA\_002151415.1)**.

-----

## Pipeline Steps

1.  **K-mer Counting:** Ran `k-mer-count/run_kmer_count.sh` for each genome.
2.  **K-mer Sorting:** Ran `k-mer-count/lexo_sorting.sh` to generate `lexo_mer_counts_PAM_sorted.txt`.
3.  **HAWK Compilation:** Compiled the custom HAWK tool:
    ```bash
    g++ -O3 -std=c++17 hawk.cpp -o hawk -lpthread
    ```
4.  **HAWK Execution:** Executed HAWK to find k-mer differences:
    ```bash
    ./hawk case_sorted_count.txt control_sorted_count.txt
    ```
5.  **Exon Extraction (gffread):** Extracted exon sequences from the annotated genome:
    ```bash
    gffread genomic.gff -g genome.fna -x exons.fa
    ```
6.  **Exon Indexing (Bowtie2):** Built an index of the extracted exons:
    ```bash
    bowtie2-build rice_exons.fa rice_exons
    ```
7.  **Unique gRNA Filtering:** Executed the custom script for unique gRNA identification and convereted it to .fasta format:
    ```bash
    gergRNAs_unique_case.sh
    ```
8. **gRNA Alignment using BowTie2:** Aligned the k-mers present multiple times in case and single times in control (reference). Also performed it for the k-mers generated from the previous step. 

    ```bash 
    .\runBowTie.sh
    .\runBowTie_unique.sh
    ```

9. **Target Gene Extraction and Quantification:** Executed `getGeneList_multiple_v_single_all` to analyze final specific targets, comparing two sets (`multiple_v_single` and `unique_PAM_gRNAs_n22`). 

    ```bash 
      .\getGeneList_multiple_v_single_all
    ```

-----