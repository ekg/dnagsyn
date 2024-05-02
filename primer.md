# primer design

Goal: multi-purpose oligo that will get synthesized with various end chemistries for integration into the GFET microfluidic platform.

Basic idea is to have several overlapping Cas9-targetable sites with nested restriction sites.
This lets us cut the DNA in a lot of places.
Cutting with type IIS endonucleases like FokI, Esp3I (BsmBI), and Eco31I (BsaI) will cleave anything we've added onto the 3' end of the molecule, and will work on ssDNA.

```txt
5' GATTACACAGGCAAGCTTAGGATGTAGGTCTTAACTCGTCTCTGGTCTCA 3'
   |-----| |-| |----| |---| |-| |----| |----| |----|
   GATTACA NGG AAGCTT GGATG NGG CTTAAC CGTCTC GGTCTC #<-- cuts after end of oligo 
           PAM Hind3  FokI  PAM spacer Esp3I  Eco31I
              |---20bp Guide 1---|
                               |---20bp Guide 2---|
```

I manually filled in the gaps to balance sequence composition and avoid any △G-favorable folds.
The spacer is a near match for EcoRI (CTTAAG), but I couldn't get a positive Gibbs free energy fold without modifying it.

```bash
s=GATTACACAGGCAAGCTTAGGATGTAGGTCTTAACTCGTCTCTGGTCTCA; echo $s; echo $s | RNAfold -P DNA; echo $s | awk '{print "A:", gsub(/A/,"A"), "T:", gsub(/T/,"T"), "G:", gsub(/G/,"G"), "C:", gsub(/C/,"C"), "N:", gsub(/N/,"N")}' 
GATTACACAGGCAAGCTTAGGATGTAGGTCTTAACTCGTCTCTGGTCTCA
WARNING: stacking enthalpies not symmetric
WARNING: stacking enthalpies not symmetric
WARNING: stacking enthalpies not symmetric
WARNING: stacking enthalpies not symmetric
GAUUACACAGGCAAGCUUAGGAUGUAGGUCUUAACUCGUCUCUGGUCUCA
.................................................. (  0.00)
A: 12 T: 15 G: 12 C: 11 N: 0
```

I then put this through unafold web DNA folding. The strongest fold has Tm = 35.7 °C.
