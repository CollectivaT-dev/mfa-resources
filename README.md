# MFA Resources
This repo is currently a documentation for installation and execution of mfa

## Installation
The [official installation](https://montreal-forced-aligner.readthedocs.io/en/latest/installation.html) of MFA depends on conda, hence here is an alternative virtualenvironment version of it. Beware, the mfa needs `openfst`, `openblas`, `ngram`, `baumwelch` and `sox` which I am not installing here. 

```
virtualenv --python=python3 venv
pip install montreal-forced-aligner
```
It will give a warning about a conflicting version of numpy, hence:

```
pip uninstall numpy
pip install numpy==1.20.3
```
and finally:

```
pip install montreal-forced-aligner -U
pip install pynini
mfa thirdparty download
```

## Running a model without a lexicon
First download the models for the desired language:
```
mfa download acoustic turkish
mfa download g2p turkish_g2p
```

generate the lexicon from the g2p models and the text file. 
```
mfa g2p turkish_g2p judeo-esp/corpus/wav/article-1.txt judeo-esp/lexicon.txt --clean --temp_directory ./tmp/
```

Finally align the audio with the transcription
```
mfa align judeo-esp/corpus/ judeo-esp/lexicon.txt turkish . --clean --temp_directory tmp
```

## Judeo-Espanyol extra steps
```
ffmpeg -i judeo-esp/Lo-Ke-Mos-Aze-Ganar-I-Perder-El-Koronavirus-Seli-Gaon.mp3 -ac 1 -ar 16000 judeo-esp/corpus/wav/article-1.wav
sed 's/sh/ş/g; s/dj/c/g; s/\(.*\)/\L\1/' judeo-esp/article-1-org.txt > judeo-esp/corpus/wav/article-1.txt
mfa g2p turkish_g2p judeo-esp/corpus/wav/article-1.txt judeo-esp/lexicon.txt --clean --temp_directory ./tmp/
mfa align judeo-esp/corpus/ judeo-esp/lexicon.txt turkish . --clean --temp_directory tmp
```
