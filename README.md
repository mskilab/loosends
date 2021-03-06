# loosends
Loose ends in cancer genome structure

This package is dependent on the directory of reference genomes and sequences found at [https://mskilab.s3.amazonaws.com/hg19/WGS/hg19_looseends.tar.gz](https://mskilab.s3.amazonaws.com/hg19/WGS/hg19_looseends.tar.gz), which is expected by default to be unzipped within the extdata/ directory (not required; alternate path can be provided)

---

## Recommended use

### `ggraph.loose.ends`

To characterize all loose ends in a single genome graph `gg`:
```{r}
ggraph.loose.ends(gg = gG(jabba=/path/to/jabba.rds),
    cov.rds = /path/to/cov.rds,
    tbam = /path/to/tumor.bam,
    nbam = /path/to/normal.bam, # optional but recommended
    id = "SAMPLE-ID",
    outdir = /path/to/output/, # default = NULL will not write output files
    purity = NULL, # default assumes purity = 1
    ploidy = NULL, # default estimates ploidy from gGraph
    field = "ratio", # normalized coverage field in cov.rds
    PTHRESH = 3.4e-7, # recommended for 200bp bins. for 1kbp bins, use 2e-6
    verbose = F,
    mc.cores = 1,
    ref_dir = system.file("extdata", "hg19_looseends", package = "loosends"))
```
- will perform quality filters on fitted loose ends and evaluate all true positives
- returns data.table describing the categorization and repeat content of each true positive loose end

Performance depends on loose end burden in graph and sequencing coverage depth

### `process.loose.ends`

To characterize a set of loose ends of interest `le`:
```{r}
process.loose.ends(le = GRanges(),
    tbam = /path/to/tumor.bam,
    nbam = /path/to/normal.bam,
    id = "SAMPLE-ID",
    outdir = /path/to/output/,
    mc.cores = 1, 
    ref_dir = system.file("extdata", "hg19_looseends", package = "loosends"), 
    verbose = FALSE)
```
- returns data.table describing the categorization and repeat content of every input loose end
- tbam, nbam, and id must all either be length=1 (all loose ends from a single sample) or length=length(le)

Performance depends on sequencing coverage depth
