## Train
```bash
sphinx_fe -argfile en-us/feat.params \
        -samprate 16000 -c arctic20.fileids \
       -di . -do . -ei wav -eo mfc -mswav yes
```
```bash
./bw \
   -hmmdir en-us \
   -moddeffn en-us/mdef \
   -ts2cbfn .ptm. \
   -feat 1s_c_d_dd \``
   -svspec 0-12/13-25/26-38 \
   -cmn current \
   -agc none \
   -dictfn cmudict-en-us.dict \
   -ctlfn arctic20.fileids \
   -lsnfn arctic20.transcription \
   -accumdir .
```
```bash
cp -a en-us en-us-tsubasa
```
```bash
./map_adapt \
    -moddeffn en-us/mdef \
    -ts2cbfn .ptm. \
    -meanfn en-us/means \
    -varfn en-us/variances \
    -mixwfn en-us/mixture_weights \
    -tmatfn en-us/transition_matrices \
    -accumdir . \
    -mapmeanfn en-us-tsubasa/means \
    -mapvarfn en-us-tsubasa/variances \
    -mapmixwfn en-us-tsubasa/mixture_weights \
    -maptmatfn en-us-tsubasa/transition_matrices
```
## Test
```bash
pocketsphinx_continuous -hmm en-us-tsubasa -lm en-us.lm.bin -dict cmudict-en-us.dict -infile 1.wav > 1.txt
```
