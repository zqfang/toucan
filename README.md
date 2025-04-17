# Toucan

## Overview

Toucan is an app that allows you visualize your single cell data in a desktop PC.

[Download](https://github.com/zqfang/toucan/releases)

## Features

- [x] Visualize single cell data: scRNA-seq, scATAC-seq, 10x Visium, NanoString Cosmx, etc
- [x] Visualize anndata file: hdf5, zarr
- [x] No Python or R runtime is required
- [x] Out-of-core, serverless, and data stays on your device.  
- [x] Cross-platform: Windows, macOS, Linux, Android, iOS
- [x] Fast Wilcoxon test (Rust implementation of [wilcoxauc](https://github.com/immunogenomics/presto) )

## Input Requirements

### Anndata

Toucan supports the following on-disk formats:

- anndata-hdf5
- anndata-zarr (recommended, speed++)


For seurat object, you can convert it to anndata-hdf5 with the following commands:

```R
# in R 
remotes::install_github("scverse/anndataR")
adata = anndataR::from_Seurat(seurat_obj)
adata$write_h5ad("data.h5ad")
# or 
remotes::install_github("zqfang/MuDataSeurat")
MuDataSeurat::WriteH5AD(seurat_obj, file = "data.h5ad", sparse.type='csc_matrx', scale.data = FALSE)
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


## Installation

1. Install hdf5 C library
   1. On MacOS, you can install it via: 

```bash
brew install hdf5@1.10
```

   2. On Linux, you can install it via: 

```bash
sudo apt install libhdf5-dev
```

   3. On Windows, no extra installation needed.
      
```bash
# not required
winget install --id HDFGroup.HDF5 --source winget`
```
2. [Download](https://github.com/zqfang/toucan/releases) the precompile binaries and install.
3. Run the app



## Credits

The frontend UI is taken from from [Cirrocumulus](https://github.com/lilab-bcb/cirrocumulus) with substantial amount of changes to adpt to the needs of Toucan.
