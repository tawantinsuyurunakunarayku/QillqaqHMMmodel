## Quechua ASR using HMM
### PUCP, Lima, Peru


#### IMPORTANT FOREWORD
------------------------------------------------------------------------------------
To be able to train/test this ASR system, you need to download the corpora available on https://siminchikkunarayku.pe/.

- Once acquired, the wav files must be copied in *data/{train,dev,test}/wav* directories following the protocol/organization mentionned in *data/{train,dev,test}.


#### OVERVIEW

The package contains quechua speech corpus with audio data in the directory *data/*.  
The data directory contains 3 subdirectories:   
1. **train/** - speech data and transription for training automatic speech recognition system
2. **dev/** - speech data and transcription for testing automatic speech recognition system    
3. **test/** - speech data and transription for evaluating automatic speech recognition system   

Some parameters that you need for HTK scripts in the directory *parameter/*, a dictionary and language model in the directory *dictionary/*, an acoustic model pre-trained in the directory *model/*, the HTK script in *htk-scripts/*.

#### PUBLICATION ON QUECHUA SPEECH & LM DATA
------------------------------------------------------------------------------------
More details on the corpus and how it was collected can be found on the following publication (please cite this bibtex if you use this data).

@InProceedings{ZEVALLOS18.4,
  author = {Rodolfo Zevallos and Luis Camacho},
  title = {Siminchik: A Speech Corpus for Preservation of Southern Quechua},
  booktitle = {Proceedings of the Eleventh International Conference on Language Resources and Evaluation (LREC 2018)},
  year = {2018},
  month = {may},
  date = {7-12},
  location = {Miyazaki, Japan},
  editor = {Ineke Schuurman and Leen Sevens and Victoria Yaneva and John O’Flaherty},
  publisher = {European Language Resources Association (ELRA)},
  address = {Paris, France},
  isbn = {979-10-95546-12-2},
  language = {english}
}

### QUECHUA SPEECH CORPUS
------------------------------------------------------------------------------------
 - *Directory:* /data/train    
######Description: training data set (about 77 hours)
**Files:** txt (training transcription), wav (empty but must contains .wav files), trsTrain.txt (training transcription and wav file id)
**For more information about the format, please refer to htk website http://htk.eng.cam.ac.uk/**       

 - *Directory:* /data/dev    
######Description: evaluation data set (about 10 hour)    
**Files:** text (dev transcription), wav (empty but must contains .wav files).         

 - *Directory:* /data/test     
######Description: testing data set (about 10 hour)     
**Files:** text (test transcription), wav (empty but must contains .wav files).     

Note: 

 
### LEXICON/PRONUNCIATION DICTIONARY
------------------------------------------------------------------------------------
 - *Directory:* dictionary/
**Files:** grammar.wordnet (LM htk format), lexicon.txt (dictionary), phones.dic (phone dictionary)
Description: lexicon sample contains words and their respective pronunciation, non-speech sound and noise in htk format.    
*Note: The lexicon no including tone and vowel length information.*


### SCRIPTS
------------------------------------------------------------------------------------
In *htk-script/* you find the scripts used to make a dictonary (htk format), LM (htk format), train and test models    
(path has to be changed to make it works in your own directory!)    
 

### EXPERIEMENT APPROACH

Each step of the experiment are described  in [REPORT.MD](https://github.com/nyro22/asr_hmm/blob/master/Report.md)

* In order to build the Quechua Language recognation system, we need first to **prepare the data** for both training and testing. We need first to create a grammar for the system, wich will represent the chain of a spoken words beginning and ending with a silence `sil`. We will use also the [Quechua language dictionary](https://siminchikkunarayku.pe/), this dictionnary will allow to create a list of diphones representing each word. We will have also to extract the features of each recording from the `train`, `dev`, `test`  datasets, using the `HCompV` command.

* In a second time, we will train a **monophone** Hidden Markov Model. The model will be re-estimate after introducing short pauses into the transcription in order to increase it's accuracy. We will also use the `HVite` command to align the phones with the training data.

Once the model is trained, it's possible to validation and evaluates and his performance against the `dev` and `test` dataset.

------------------------------------------------------------------------------------
##### These results were obtained from the HTK's version of October 2017

Acoustic models                | WER score *(%)* on **Initial** set   |
:------------------------------|:------------------------------------:| 
Monophone Aligned *(13 MFCC)*  |                14.52                 |
Monophone Tied    *(13 MFCC)*  |                19.60                 |


# Contact
We are always happy to hear from you:

rjzevallos.salazar@gmail.com \
camacho.l@pucp.pe
=======
# Qillqaq HMM model
HMM model to build a ASR for quechua language

