# Forced Aligner for Panãra (Brazil)

Emily P. Ahn, Bridget Tyree, Myriam Lapierre (University of Washington Linguistics)

Contact `{eahn|mylapier}@uw.edu`

## Requirements
* conda
* python 3.X
* Montreal Forced Aligner (MFA) via conda (see [instructions](https://montreal-forced-aligner.readthedocs.io/en/latest/getting_started.html) for both conda and MFA installation)
* Interlingual MFA [Github](https://github.com/jhdeov/interlingual-MFA) for Option 2 below
	* tgt (TextGrid tools Python module), install with `pip install tgt` 

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
	* Note: I (Emily) have decided to make 'words' the same as 'phones'.
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

Note: as of MFA version 2.2.11 on Emily's laptop, there is a database shutdown error thrown at the end of the alignment process. This doesn't affect the alignment output. See github [issue](https://github.com/MontrealCorpusTools/Montreal-Forced-Aligner/issues/640). Should be fixed in current versions.

##### Option 1: Panara-trained aligner
* (Easier to run, but performs worse)
* Trained on ~30-45min of Panara interview speech

After initializinzg/activating conda, run
`mfa align [IN_DIR] models/panara_only_dict.txt models/panara_only.zip [OUT_DIR]`

NOTES: OUT_DIR does not need to be initalized, and paths do not have to be absolute

##### Option 2: Global English-trained + Panara-adapted aligner
* (More complicated to run, but performs better)
* Trained on 3900 hours of Global English from areas including US, Nigeria, India (see [info](https://mfa-models.readthedocs.io/en/latest/acoustic/English/English%20MFA%20acoustic%20model%20v2_2_1.html)), then adapted/fine-tuned on Panara speech (the same 30-45min from Option 1)

1. Clone the git repository for Interlingual-MFA (see link above under Requirements) in a _sister folder outside of this repostiory_, so there aren't weird syncing issues.
1. Use our phone mapping file to create intermediate dictionary. (This also creates a pickle file in the top-level dir.) `python ../interlingual-MFA/convertPronDict.py models/panara_only_dict.txt models/phoneMapping_English.txt models/pronDictIntermediate_Eng-Pan.txt`
	* Note: if you have additional phones not in this phone mapping file, feel free to add extra lines to `models/phoneMapping_English.txt` that look like `{panara_phone}\t{eng_phone}`. To see the list of Eng phones, see the dictionary [here](https://mfa-models.readthedocs.io/en/latest/dictionary/English/English%20MFA%20dictionary%20v2_2_1.html#english-mfa-dictionary-v2-2-1)
1. Run MFA: `mfa align [IN_DIR] models/pronDictIntermediate_Eng-Pan.txt models/panara_only.zip [OUT_DIR]`
1. Convert output TextGrids back to the original phone-set using the pickle file from the output of 2 steps above: `python ../interlingual-MFA/convertAlignments.py wordTranscriptions.pkl [OUT_DIR]`

NOTE: Step 2 above can probably be skipped after you generate the pickle file for the first time. 

