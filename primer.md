# primer design

Goal: multi-purpose oligo that will get synthesized with various end chemistries for integration into the GFET microfluidic platform.

Basic idea is to have several overlapping Cas9-targetable sites with nested restriction sites.
This lets us cut the DNA in a lot of places.
Cutting with type IIS endonucleases like FokI, Esp3I (BsmBI), and Eco31I (BsaI) will cleave anything we've added onto the 3' end of the molecule, and will work on ssDNA.

```txt
5' GATTACACAGGCAAGCTTGGGATGTAGGTCTTAACTCGTCTCTGGTCTCA 3'
   |-----| |-| |----| |---| |-| |----| |----| |----|
   GATTACA NGG AAGCTT GGATG NGG CTTAAG CGTCTC GGTCTC #<-- cuts after end of oligo 
           PAM Hind3  FokI  PAM ECoRI  Esp3I  Eco31I
              |---20bp Guide 1---|
                               |---20bp Guide 2---|
```

I manually filled in the Ns to balance sequence composition and avoid any △G-favorable folds:

```bash
x=GATTACANNGGNAAGCTTNGGATGNNGGNCTTAAGNCGTCTCNGGTCTCN; s=GATTACACAGGCAAGCTTGGGATGTAGGTCTTAACTCGTCTCTGGTCTCA; echo $x; echo $s; echo $s | RNAfold -P DNA; echo $s | awk '{print "A:", gsub(/A/,"A"), "T:", gsub(/T/,"T"), "G:", gsub(/G/,"G"), "C:", gsub(/C/,"C"), "N:", gsub(/N/,"N")}'
GATTACANNGGNAAGCTTNGGATGNNGGNCTTAAGNCGTCTCNGGTCTCN
GATTACACAGGCAAGCTTGGGATGTAGGTCTTAACTCGTCTCTGGTCTCA
WARNING: stacking enthalpies not symmetric
WARNING: stacking enthalpies not symmetric
WARNING: stacking enthalpies not symmetric
WARNING: stacking enthalpies not symmetric
GAUUACACAGGCAAGCUUGGGAUGUAGGUCUUAACUCGUCUCUGGUCUCA
.................................................. (  0.00)
A: 11 T: 15 G: 13 C: 11 N: 0
```

I'm not sure about the warnings but this is better than the first iteration I ran which had a △G of -10 and was very likely to form a hairpin.
The idea here is that the DNA should be basically linear with little internal interaction to reduce binding efficiency of whatever we throw at it.
