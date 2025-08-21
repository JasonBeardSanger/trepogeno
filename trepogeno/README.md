Typing scheme rule book.

As a required input when preforming custom lineage calling is a typing scheme which mykrobe sometimes refers to a reference coordinate file.
This typing scheme is used to define which Alleles at which bases define which lineages.
Mykobe takes this scheme and for each SNP generates a kmer probe and an entry in a json which it uses to make calls and look up which lineage that call belongs to.

Mykrobe has some very specific rules when it comes to desgining a typing scheme which can cause some difficulties.

A normal typing scheme will be a tsv structed like this wihtout any headings:

```
ref	6442	A	G	DNA	TPA.2
ref	6654	T	C	DNA	TPE
ref	6740	A	G	DNA	*TPA.1.6
```

The first column must be 'ref'
The second column is the location base pair 
The third column is the base found in the reference, the reference allele
The fourth column is the base found alternatively, the alternative allele
The fith column must be DNA, mykrobe can also define lineages by amino acid changes within genes, this isn't used here.
The sixth column is the lineage in which the given SNP can be used to segregate

A * must be used for lineages in which the reference genome belongs, as can be seen in the above example.
Normally mykrobe assumes a lineage is defined by the presence of the alternative allele.
The * tells mykrobe the lineage is instead defined by the presence of the reference allele.

Additioanlly, you can not have a SNP at a base descriminate for multiple diverging lineages.
For example, this is an invalid scheme:
```
ref	4800	C	A	DNA	TPA.2.7
ref	4800	C	A	DNA	TPA.2.9
```

Mykrobe creates a key value pair from the base and change like this C4800A:TPA.2.9. It uses this to match the probes to lineages.
This method doesn't natively support multiple of the same key.
Having duplicates of the same base position like above will cause myrkobe to still only create one key while the others are be discarded.
Importantly if that change at that base is actually discrimatory for both diverging lineages it will only ever call one of them from the resulting probe.

Finally you must ensure that your scheme has defined SNPS for every level of your hierarchy.
If you wanted to define SNPS for the lineages TPA.2.7 and TPE.3.1 like so:
```
ref	213784	A	G	DNA	TPA.2.7
ref	251918	G	A	DNA	TPA.2.7
ref	266587	T	C	DNA	TPE.3.1
ref	470932	C	T	DNA	TPE.3.1
```

Then you MUST also include SNPS in your scheme for TPA, TPA.2, TPE, and TPE.3 like so:

```
ref	20770	A	G	DNA	TPA
ref	6442	A	G	DNA	TPA.2
ref	88317	T	C	DNA	TPE.3
ref	90092	G	T	DNA	TPE
```

If mykrobe is unable to find at least some support for all three level of this hierarchy it won't call it. 