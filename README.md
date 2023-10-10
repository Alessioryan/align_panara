# Forced Aligner for Panãra (Brazil)

Emily P. Ahn, Bridget Tyree, Myriam Lapierre (University of Washington Linguistics)

Contact `{eahn|mylapier}@uw.edu`

## Requirements
* conda
* python 3.X
* Montreal Forced Aligner (MFA) via conda
* Interlingual MFA [Github](https://github.com/jhdeov/interlingual-MFA)
* etc

## Steps

### Grapheme-to-Phone(me) Conversion
* TBD

### Forced Alignment

#### Input data format
* Audio in .wav
* Text in either `.lab/.txt` or `.TextGrid`
	* if you have the full phone sequence over the entire wav file, you can just paste that phone sequence into a text file and give it the extension `.lab` or `.txt`
	* if you have a phone sequence only over _part_ of the wav file (i.e. there's other speech that is not phone-transcribed), you must create a Praat TextGrid (`.TextGrid`) with an interval tier that has an interval labeled with that phone sequence. You can also have multiple intervals with their own phone sequences within a single TextGrid.
	* phone sequence should be space-separated, e.g. `k i t a`
	* 
* NOTE: the audio and corresponding text file must have the same name before the file extension, e.g. `spkr1_item4.wav & spkr1_item4.txt`
* Dictionary file (use aligner-corresponding file under `models/`)
* See MFA [documentation](https://montreal-forced-aligner.readthedocs.io/en/latest/user_guide/corpus_structure.html)
	* includes how to specify different speakers

#### Input data file structure
* Corresponding wav and text files should be in the same folder
* See MFA [documentation](https://montreal-forced-aligner.readthedocs.io/en/latest/user_guide/corpus_structure.html)
	* discusses how you can specify different speakers with file structure


#### Run MFA

General steps:
1. initialize conda environment for MFA: `conda activate aligner`
1. `mfa align [IN_DIR] [DICTIONARY] [MODEL.zip] [OUT_DIR]`

##### Option 1: Panara-trained aligner
* (Easier to run, but performs worse)
* Trained on ~30-45min of Panara interview speech


##### Option 2: Global English-trained + Panara-adapted aligner
* (More complicated to run, but performs better)
* Trained on 3900 hours of Global English from areas including US, Nigeria, India (see [info](https://mfa-models.readthedocs.io/en/latest/acoustic/English/English%20MFA%20acoustic%20model%20v2_2_1.html)), then adapted/fine-tuned on Panara speech (same as above)

TBD
