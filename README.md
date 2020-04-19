## Auto Train Script
```bash
#!/bin/bash

for i in {1..20};       # Train 20 times
do sphinx_fe -argfile [YOUR_MODEL_DIR]/feat.params \
        -samprate 16000 -c [YOUR_FILEIDS] \
       -di . -do . -ei wav -eo mfc -mswav yes;
./bw \
	-hmmdir [YOUR_MODEL_DIR] \
	-moddeffn [YOUR_MODEL_DIR]/mdef \
   -ts2cbfn .ptm. \
   -feat 1s_c_d_dd \
   -svspec 0-12/13-25/26-38 \
   -cmn current \
   -agc none \
   -dictfn [YOUR_DICT] \
   -ctlfn [YOUR_FILEIDS] \
   -lsnfn [YOUR_TRANSCRIPTION] \
   -accumdir .;
cp -a [YOUR_MODEL_DIR] [YOUR_MODEL_DIR]-tmp;
./map_adapt \
	-moddeffn [YOUR_MODEL_DIR]/mdef \
    -ts2cbfn .ptm. \
	-meanfn [YOUR_MODEL_DIR]/means \
	-varfn [YOUR_MODEL_DIR]/variances \
	-mixwfn [YOUR_MODEL_DIR]/mixture_weights \
	-tmatfn [YOUR_MODEL_DIR]/transition_matrices \
    -accumdir . \
    -mapmeanfn [YOUR_MODEL_DIR]-tmp/means \
    -mapvarfn [YOUR_MODEL_DIR]-tmp/variances \
    -mapmixwfn [YOUR_MODEL_DIR]-tmp/mixture_weights \
    -maptmatfn [YOUR_MODEL_DIR]-tmp/transition_matrices;
rm [YOUR_MODEL_DIR] -rf;
mv [YOUR_MODEL_DIR]-tmp [YOUR_MODEL_DIR];
done;
```
## Test
```bash
pocketsphinx_continuous -hmm [YOUR_MODEL_DIR] -lm [YOUR_LANGUAGE_MODEL] -dict [YOUR_DICT] -infile [AUDIO_FILE] > [AUDIO_FILE].txt;
```
