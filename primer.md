# primer design

Basic idea is to have several overlapping Cas9-targetable sites with nested restriction sites.
This lets us cut the DNA in a lot of places.


```txt
5' GATTACACAGGCAAGCTTAGGATGACGGACTTAAGTCGTCTCCGGTCTCA 3'
   |-----| |-| |----| |---| |-| |----| |---|  |----|
   GATTACA NGG AAGCTT GGATG NGG CTTAAG CGTCTC GGTCTCN
           PAM Hind3  FokI  PAM ECoRI  Esp3I  Eco31I
              |---20bp Guide 1---|
                               |---20bp Guide 2---|
```

I manually filled in the Ns to balance sequence composition using this script:

```bash
echo GATTACACAGGCAAGCTTAGGATGACGGACTTAAGTCGTCTCCGGTCTCA | awk '{print "A:", gsub(/A/,"A"), "T:", gsub(/T/,"T"), "G:", gsub(/G/,"G"), "C:", gsub(/C/,"C"), "N:", gsub(/N/,"N")}'
A: 13 T: 12 G: 13 C: 12 N: 0
```
