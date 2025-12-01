

# 🧬 Krispmer Experimentation Pipeline - Progress 🔬

## 📊 Datasets Used

| Name | Link |
| :--- | :--- |
| **Genome Assembly Rice\_IR8\_v1.7** | [https://www.ncbi.nlm.nih.gov/datasets/genome/GCA\_001889745.1/](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_001889745.1/) |
| **Genome Assembly Rice\_N22** | [https://plants.ensembl.org/Oryza\_sativa\_n22/Info/Index](https://plants.ensembl.org/Oryza_sativa_n22/Info/Index) |
| **Genome Assembly Basmati** | Stored before in the VM|
| **Genome Assembly Rice\_Japonica (annotated)** | [https://www.ncbi.nlm.nih.gov/datasets/genome/GCF\_034140825.1/](https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_034140825.1/) |
| **Genome assembly R498.Genome.version1 (Indica reference + annotated)** | [https://www.ncbi.nlm.nih.gov/datasets/genome/GCA\_002151415.1/](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_002151415.1/) |

-----

## 🌾 Reference Genome Choice

All compared genomes (Basmati, IR8, N22) are from **Indica rice**, thus the **Indica R498 reference genome (GCA\_002151415.1)** was selected for difference analysis.

-----

## 🛠️ Pipeline Steps (Chronological Execution)

1.  **K-mer Counting:** Executed Jellyfish counting:

    ```bash
    k-mer-count/run_kmer_count.sh
    ```

2.  **K-mer Sorting:** Sorted k-mer counts lexicographically:

    ```bash
    k-mer-count/lexo_sorting.sh
    ```

3.  **HAWK Compilation:** Compiled the custom difference-finding tool:

    ```bash
    g++ -O3 -std=c++17 hawk.cpp -o hawk -lpthread
    ```

4.  **HAWK Execution:** Compared 'case' vs. 'control' sorted k-mer lists:

    ```bash
    ./hawk case_sorted_count.txt control_sorted_count.txt
    ```

5.  **Exon Extraction:** Used `gffread` to extract exon sequences for indexing:

    ```bash
    gffread genomic.gff -g genome.fna -x exons.fa
    ```

6.  **Exon Indexing:** Built a Bowtie2 index of the reference exons:

    ```bash
    bowtie2-build rice_exons.fa rice_exons
    ```

7.  **Unique gRNA Generation:** Executed script to filter HAWK output and convert unique candidate k-mers (potential gRNAs) to FASTA format:

    ```bash
    gergRNAs_unique_case.sh
    ```

8.  **gRNA Alignment and Filtering:** Aligned candidate gRNAs against the exon index and filtered for unique hits:

    ```bash
    .\runBowTie.sh
    .\runBowTie_unique.sh # Filters SAM/BAM output for highly specific targets
    ```

9.  **Target Gene Extraction:** Executed final script to identify and quantify the unique **gene IDs** targeted by the perfectly-matching (XM:i:0) gRNAs:

    ```bash
    .\getGeneList_multiple_v_single_all
    ```

-----

