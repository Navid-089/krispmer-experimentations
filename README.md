# 🧬 Krispmer Experimentation Pipeline - Progress

##  Datasets Used

| Name                                                                    | Link                                                                                                                           |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **Genome Assembly Rice_IR8_v1.7**                                       | [https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_001889745.1/](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_001889745.1/) |
| **Genome Assembly Rice_N22**                                            | [https://plants.ensembl.org/Oryza_sativa_n22/Info/Index](https://plants.ensembl.org/Oryza_sativa_n22/Info/Index)               |
| **Genome Assembly Basmati**                           | Stored before in the VM|
| **Genome Assembly Rice_Japonica (annotated)**                           | [https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_034140825.1/](https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_034140825.1/) |
| **Genome assembly R498.Genome.version1 (Indica reference + annotated)** | [https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_002151415.1/](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_002151415.1/) |



## Reference Genome Choice

All compared genomes (Basmati, IR8, N22) are from **Indica rice**, so we selected the **Indica R498 reference genome (GCA_002151415.1)**.



## Step-1: K-mer Counting Workflow

For each genome, we executed:

1. **K-mer counting**

   * Using `k-mer-count/run_kmer_count.sh` inside each genome folder.
   * Output includes:

     * `mer_counts.jf`
     * `mer_counts_PAM.txt`
     * `mer_counts_dumps.txt` (where applicable)

2. **Sorting**

   * Lexicographical sorting using `k-mer-count/lexo_sorting.sh`
   * Sorted output saved as:

     * `lexo_mer_counts_PAM_sorted.txt`

## Step-2: Custom HAWK implementation

1. **hawk-executable**

   * Put `hawk.cpp` and `kmer.h` in the same folder
   * Compile using pthreads:

     ```
     g++ -O3 -std=c++17 hawk.cpp -o hawk -lpthread
     ```

2. **Reads two sorted k-mer count filenames from**

   * `case_sorted_count.txt`
   * `control_sorted_count.txt`

3. **Run hawk**

   ```
   ./hawk case_sorted_count.txt control_sorted_count.txt
   ```

---
