# Toucan

## Overview

Toucan is an app that allows you visualize your large-scale single-cell data in a desktop PC.

[Download](https://github.com/zqfang/toucan/releases) the pre-compiled binaries.


![screenshot](./toucan.gif)


## Features

- [x] Visualize single cell data: scRNA-seq, 10x Visium, NanoString Cosmx, etc
- [x] Visualize anndata file: hdf5, zarr
- [x] No Python or R runtime is required
- [x] Out-of-core, serverless, and data stays on your device.  
- [x] Cross-platform: Windows, macOS, Linux, Android, iOS
- [x] Fast Wilcoxon test (Rust implementation of [wilcoxauc](https://github.com/immunogenomics/presto) )
- [x] scalable for millions of cells.  

## Usage

Please refer to [Cirrocumulus](https://cirrocumulus.readthedocs.io/en/latest/documentation.html)

## Installation

1. Install hdf5 C library

- On MacOS, you can install it via: 

```bash
brew install hdf5@1.10
```

after install toucan, in terminal, run
```bash
# to fix app could not open issue
sudo xattr -r -d com.apple.quarantine /Applications/Toucan.app
```

- On Linux, you can install it via: 

```bash
sudo apt install libhdf5-dev
```

- On Windows, no extra installation needed.
      
```bash
# not required
winget install --id HDFGroup.HDF5 --source winget
```
2. [Download](https://github.com/zqfang/toucan/releases) the precompile binaries and install.
3. Run the app

## Input Requirements

### Anndata

Toucan supports the following on-disk formats:

- anndata-hdf5
- anndata-zarr (recommended, speed++)


For seurat object, you can convert it to anndata-hdf5 with the following commands:

```R
# in R, install
remotes::install_github("zqfang/MuDataSeurat")
# then
MuDataSeurat::WriteH5AD(seurat_obj, 
                        file = "data.h5ad", 
                        assay="RNA",
                        sparse.type='csc_matrx', 
                        scale.data = FALSE)


# or use 
remotes::install_github("scverse/anndataR")
adata = anndataR::from_Seurat(seurat_obj, 
                      assay_name = "SCT", 
                      x_mapping = 'data', 
                      layers_mapping =list(counts = "counts"))
adata$write_h5ad("data.h5ad")
```
### Spatial Transcriptoimcs (optional)

In addition to anndata file, if you want to visualize spatial transcriptomics data, 
put the following two files in the same directory as the anndata file:

1. spatial `coordinates`: 
   - same filename prefix as the anndata file, but with `.image.positions.csv` extension.
   - the csv file needs 3 columns: `cell_id`, `x`, `y`.
2. spatial `images`: same filename prefix as the anndata file, but with `image.png/jpg` extenstion.

```
--|
  |-- data.h5ad
  |-- data.image.positions.csv
  |-- data.image.png
```

see an spatial example input in the `example` data folder


## Credits

The frontend UI is taken from from [Cirrocumulus](https://github.com/lilab-bcb/cirrocumulus) with substantial amount of changes to adpt to the needs of Toucan.
