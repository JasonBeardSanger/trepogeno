This directory contains the scripts that are used for making calls to mykrobe and for outputting the probe and taxonomy files that are latter used for lineage calling.

### Argument Example
trepogeno \
--json_directory files/json_outputs \
--type_scheme files/Tpallidum.SNP.table_hierarchies_2025-05-14.tsv \
--genomic_reference files/reference/nc_021508.fasta \
--probe_and_lineage_dir files/probes \
--make_probes \
--probe_lineage_name custom_typing 

### Output
custom_typing.fa
custom_typing.json


### run time execution:
This script is very simple and mosly consists of importing the requried function from mykrobe and executing it with our own captured arguments.
There is a little juggling with the probe.fa file as mykrobe doesn't take the probe name as a argument instead making users redirect standard out with > and saving to the current directory
Instead we use redirect_stdout from contextlib to save to a file and can then save the lineage.json and probe.fa to where ever the use supplied

## All paramaters 

``` 
-----------
--make_probes (required if you want to make probes)
    Used to indicate you wish to generate a new set of probes during the work flow

--type_scheme (required)
    Path to the file that maps snps to specific genomic coordiantes and lineages, to learn more review the mykrobe custom lineage calling documentation.

--genomic_reference (required)
    Path to a fasta file that acts as the genomic reference, must match the reference used in the typing scheme

--probe_and_lineage_dir (optional; Deafult './')
    This is the directory in which to save the probe and lineage file during probe creation

--probe_lineage_name (optional; Deafult 'probe.fa' & 'lineage.json')
    what to call the probe.fa file and lineage.json when writing an output

--kmer_size (optional; Deafult '21')
    what kmer size to use when creating the probes
```

Further details can be found here:
https://github.com/Mykrobe-tools/mykrobe/wiki/Custom-Panels
https://github.com/Mykrobe-tools/mykrobe/wiki/Custom-Lineage-Calling
