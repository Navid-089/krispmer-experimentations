# INSTALLATION GUIDE — kRISP-mER

* Use a Conda virtual environment
* Install dependencies from default channels
* Build **Jellyfish** from the official upstream GitHub
* Manually place **dna_jellyfish** into the environment's `site-packages`

This is the **authoritative installation procedure**.

---

# 1. Create Conda Environment

```bash
conda create -n krispmer python=3.8 -y
conda activate krispmer
```

---

# 2. Install Required Tools (samtools, bowtie2, Java)

These are available from default Conda channels. No special configuration needed.

```bash
conda install -y samtools bowtie2 openjdk
```

Verify:

```bash
samtools --version
bowtie2 --version
java -version
```

---

# 3. Install Python Dependencies

Install common scientific Python packages:

```bash
conda install -y numpy scipy pandas biopython scikit-learn
```

---

# 4. Build & Install Jellyfish (from official GitHub)

kRISP-mER requires the Jellyfish binary. Install it from upstream:

**Repository:** [https://github.com/gmarcais/Jellyfish](https://github.com/gmarcais/Jellyfish)

### 4.1 Clone the repository

```bash
cd ~
git clone https://github.com/gmarcais/Jellyfish.git
cd Jellyfish
```

### 4.2 Install build dependencies into the conda environment

```bash
conda install -y autoconf automake libtool make gxx_linux-64 pkg-config
```

### 4.3 Build Jellyfish into your Conda environment

```bash
autoreconf -i
./configure --prefix="$CONDA_PREFIX"
make -j 4
make install
```

### 4.4 Verify

```bash
jellyfish --version
```

---

# 5. Install kRISP-mER

Clone the repository:

```bash
cd ~
git clone https://github.com/mahmudhera/kRISP-mER.git
cd kRISP-mER
```

Install in editable mode:

```bash
python setup.py install
```

---

# 6. Copy dna_jellyfish (Required Step)

After building Jellyfish from the upstream repository, the Python binding
`dna_jellyfish` is available inside the Jellyfish source tree (typically
under a `python/` directory or generated during build).

kRISP-mER requires `import dna_jellyfish` to work. You must manually copy
this module into the active Conda environment's `site-packages`.

### 6.1 Locate Conda site-packages

```bash
python -c "import sysconfig; print(sysconfig.get_paths()['purelib'])"
```

Example output:

```
/home/user/miniconda3/envs/krispmer/lib/python3.8/site-packages
```

### 6.2 Locate dna_jellyfish inside Jellyfish build

From the Jellyfish repository you built earlier:

```bash
cd ~/Jellyfish
find . -name "dna_jellyfish*"
```

This should locate the `dna_jellyfish` directory or compiled module.

### 6.3 Copy dna_jellyfish into Conda environment

```bash
cp -r path/to/dna_jellyfish \
$(python -c "import sysconfig; print(sysconfig.get_paths()['purelib'])")/
```

Ensure the copied folder contains `__init__.py` or compiled extension files.

### 6.4 Verify import

```bash
python -c "import dna_jellyfish; print('dna_jellyfish OK')"
```

---

# 7. Final Verification

Final Verification

```bash
python -c "import dna_jellyfish, numpy, sklearn; print('Imports OK')"
jellyfish --version
krispmer --help
```

If all three commands work, installation is complete.

---

# 8. Example Usage

```bash
krispmer -nvr reads.fastq target.fasta out.csv 1
```

Ensure the input FASTQ and FASTA files exist.

---

# 9. Notes

* This guide intentionally avoids additional conda channels.
* Jellyfish is always installed from the upstream GitHub repository.
* dna_jellyfish must be copied manually because it is not available from pip or conda.

---


