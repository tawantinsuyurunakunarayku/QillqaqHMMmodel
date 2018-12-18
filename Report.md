# Report

The main object objectif of the project is to build a Quechua language recognation system based on HTK toolkit. We will use 2 datasets to train a model (`train`) and to evaluate his performance (`test`).

## Project structure

Before jumping into the explaination of the experiment, it's important to understand the code-structure of the project:

```
  ├── Parameter 	              # Configurations of HTK commands (.config)
  │   ├── HCompV
  │   ├── HCopy
  │   ├── HCopyTests
  │	  ├── HRest
  │	  ├── HHEdIncrementMixtures
  │	  ├── HLEd
  │   └── config
  ├── Data
  │   ├── train                       # Initial datasets to train
  │   │   ├── wav
  │   │   ├── mfcc
  │   │   └── lab
  │   ├── dev                         # Initial datasets to dev
  │   │   ├── wav
  │   │   ├── mfcc
  │   │   └── lab
  │   └── test                        # Initial datasets to test
  │       ├── mfcc
  │       ├── lab
  │       └── lab
  ├── Dictionary 		      # Inital data to build phones lists and dictionary    
  │   ├── dict
  │   ├── grammar
  │   ├── grammar.wordnet
  │	  ├── phones.dic
  │	  ├── phones.list
  │   └── words-sorted.list
  ├── Models                          # Generated models
  │   ├── hmm0
  │   ├── ...
  │   ├── hmm10
  │   ├── hmmlist
  │   └── prototype                   # Model template
  ├ README.md
  ├ REPORT.md
  ├── htk_script                      # Script to execute trainning and testing
  │   ├── htk_script.txt
  │                      
```

# Experiement approach

Step-by-step explanation of the experimentation.

## Data preparation

Create the dictionary and the list of phones composing each words.

```
HDMan -m -w \dictionary\words-sorted.list -n \dictionary\phones.list -l dlog \dictionary\phones.dict \dictionary\dict
```

Convert the grammar file into a wordnet.

```
HParse \dictionary\grammar \dictionary\grammar.wordnet
```

Create the file `train-phones.mlf` that it contains all transcriptions of phonemes.

```
HLEd -d \dictionary\phones.dict -i \data\train\train-phones.mlf \parameter\HLEd.config \data\train\train-words.mlf
```

Extract features from audio files for the `train` dataset.

```
HCopy -T 1 -C \parameter\HCopy.config -S \data\train\list.scp
```


## Model training

**Note**: In the training part we will use the same parameters for `HCompV`, `HERest`, `HVite`


Train the first model in `hmm0` using the `prototype` as **initial model**.

```
HCompV -C \parameter\HCompV.config -f 0.01 -m -S \data\train\listMFC.txt -M \model\hmm0 \model\prototype.txt
```

Reestimate the model the model using `HERest` command 4 times.

```
HERest -T 0 -C \parameter\HERest.config -I \data\train\train-phones.mlf -t 250.0 150.0 10000.0 -S \data\train\listMFC.txt -H \model\hmm0\macros -H \model\hmm0\hmmdefs -M \model\hmm1 \dictionary\phones.list
HERest -T 0 -C \parameter\HERest.config -I \data\train\train-phones.mlf -t 250.0 150.0 10000.0 -S \data\train\listMFC.txt -H \model\hmm1\macros -H \model\hmm1\hmmdefs -M \model\hmm2 \dictionary\phones.list
HERest -T 0 -C \parameter\HERest.config -I \data\train\train-phones.mlf -t 250.0 150.0 10000.0 -S \data\train\listMFC.txt -H \model\hmm2\macros -H \model\hmm2\hmmdefs -M \model\hmm3 \dictionary\phones.list
HERest -T 0 -C \parameter\HERest.config -I \data\train\train-phones.mlf -t 250.0 150.0 10000.0 -S \data\train\listMFC.txt -H \model\hmm3\macros -H \model\hmm3\hmmdefs -M \model\hmm4 \dictionary\phones.list
```

**Gaussian mixtures**

```
HHEd -H D:\htk\model\hmm4\macros -H D:\htk\model\hmm4\hmmdefs -M D:\htk\model\hmm5 D:\htk\parameter\HHEdIncrementMixtures.conf D:\htk\dictionary\phones.list
```

Reestimate the model 5 times using the transcription containing short pauses.

```
HERest -T 0 -C \parameter\HERest.config -I \data\train\train-phones.mlf -t 250.0 150.0 10000.0 -S \data\train\listMFC.txt -H \model\hmm5\macros -H \model\hmm5\hmmdefs -M \model\hmm6 \dictionary\phones.list
HERest -T 0 -C \parameter\HERest.config -I \data\train\train-phones.mlf -t 250.0 150.0 10000.0 -S \data\train\listMFC.txt -H \model\hmm6\macros -H \model\hmm6\hmmdefs -M \model\hmm7 \dictionary\phones.list
HERest -T 0 -C \parameter\HERest.config -I \data\train\train-phones.mlf -t 250.0 150.0 10000.0 -S \data\train\listMFC.txt -H \model\hmm7\macros -H \model\hmm7\hmmdefs -M \model\hmm8 \dictionary\phones.list
HERest -T 0 -C \parameter\HERest.config -I \data\train\train-phones.mlf -t 250.0 150.0 10000.0 -S \data\train\listMFC.txt -H \model\hmm8\macros -H \model\hmm8\hmmdefs -M \model\hmm9 \dictionary\phones.list
HERest -T 0 -C \parameter\HERest.config -I \data\train\train-phones.mlf -t 250.0 150.0 10000.0 -S \data\train\listMFC.txt -H \model\hmm9\macros -H \model\hmm9\hmmdefs -M \model\hmm10 \dictionary\phones.list
```

**Ally!!!** The model has been trained!

## Testing
