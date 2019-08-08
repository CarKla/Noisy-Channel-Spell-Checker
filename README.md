# Noisy-Channel-Spell-Checker
A tool for correcting misspellings in textual input using the Noisy Channel Model.

## Usage
- [ ] Clone the repository 
- [ ] Install the SRILM toolkit (http://www.speech.sri.com/projects/srilm/download.html)
- [ ] set variable SRILM_PATH in ``spell-checker.py`` to your SRILM binary folder
   (e.g. ``/user/srilm/bin/i686-m64``)
- [ ] Run ``python3 spell-checker [args]``

We recommend to use a Linux operating system. On Windows systems it is also possible by porting it using Cygwin.

## Command Line Parameters
The tool can be executed by calling ``spell-checker.py``. It provides a variety of parameters for controlling different aspects.

| **Argument**        |    **Value**        | **Description**  |
| :-------------: |:-------------:|:-----:|
| **`-c`** or **`--correct`**       | file \| directory \| Standard Input | Text that is supposed to be corrected by the spell checker. You can enter one or more files, multiple directories or direct input. Directories are recursively traversed  |
| **`-o`** or **`--output`**       | file \| directory  | Determine where to store the corrected files (per default: output/) |
| **`-ow`** or **`--overwrite`**       |  | If set, all the selected documents are overwritten by its correction. |
|      | |  |
| **`-lm`** or **`--languagemodel`**       | lm.arpa | TODO: (kombi mit arpa -> datei lesen)Filename to determine where to store the trained, arpa-formated language model. |
| **`-tr`** or **`--train`**       | file \| directory | Training files to train a language model. You can enter file(s) or entire folder(s). |
| **`-ocr`**       |  | Use tool to post-process noisy OCR texts. **Attention:** You can either use the -ocr or the -typo option.  |
| **`-typo`**       |  | Use tool to correct texts containing typos. |
|      | |  |
| **`--lmweight`**  | float number |  numeric value *w*  that weights the language model |
| **`--order`**        | Integer | Order of generated language model. Default: 2-gram |
| **`--error_model_smooth`**| float number | Pseudocount for laplace smoothing of the error model probabilities. |
| **`-sw`** or **`--stopwords`**       | stopwords.file \| direct input | List of stopwords being ignored during correction |
|      | |  |
| **`-v`** or **`--version`**       |  | Prints version of the Noisy Channel Spell Checker. |
| **`-q`** or **`--quiet`**       | | Suppress printouts. |
| **`-vb`** or **`--verbose`**       |  |  Print verbose. |
|      | |  |
| **`-te`** or **--test`**      | | Evaluates the spell checker on the sample documents from the Royal Society Corpus |
| **`--royal`**      | | Correct the entire Royal Society Corpus |
| **`-ppl`** or **`--perplexity`**       | textfile, languagemodel.arpa \| Standard Input | Computes the Perplexity measure for given file and language model |
| **`--num_cores`** or **`-cores`**       | Integer | Number of cores that can be used for computations, default: N - 1  |


## Tutorial

#### Train a Language Model from scratch
The n-gram **order** has to be specified a priori. The **train** parameter gets the textual input for training the language model. It supports a sequence of files or folders. Beside ordinary text files the parser supports well-structured xml-files and xml.tagged-files are verticalized xml-files annotated by the TreeTagger (see references). The SRILM toolkit generates the language model and stores it with location and name specified with the parameter **lm**.     
 
```ps
python spell-checker.py --order 2 --train data/corporaTagged/ -lm LM2.arpa
```
#### Core functionality: correct misspelled texts
The spell checker can **correct** files specified in the command line. Again it supports files and directories. Here we are using a bigram Language Model *LM2.arpa* which is placed in the *data/* folder. By default the corrected files are stored in the *output/* folder with a ```_corrected``` suffix
```ps
python spell-checker.py --order 2 --correct file.txt directory/ -lm data/LM2.arpa 
```
#### Evaluate the model
The package offers a test set of misspelled texts with their corresponding ground truth. This test set is a small subset of the Royal Society Corpus. The parameter **test** first processes the 26 test files and then compares the result against the ground truth. Depending on your machine and the number of cores you are using this can take a couple of minutes.
```ps
python spell-checker.py -lm data/LM2.arpa --test --order 2
```
#### Special Use Case: Post-Process the Royal Society Corpus
You only need to specify the parameter **royal** and the process starts. The corpus comprise of approximately 10.000 files that is the entire procedure takes multiple hours. 
**Prerequirement**: Inside *data/* you need a folder *corpusTagged/*. It contains the files in *train_data.txt* in a verticalized format. You should also create a folder *CorrectedCorpus/*. This is the destination folder for the processed Royal Society Corpus. We recommend a **2-gram** language model. 
```ps
python spell-checker.py -lm data/LM2.arpa --royal --order 2
```

##  References

* **Hannah Kermes, Stefania Degaetano-Ortlieb, Ashraf Khamis, J&ouml;rg Knappen, and Elke Teich.** *The Royal Society Corpus: From Uncharted Data to Corpus.* In Proceedings of the Tenth International Conference on Language Resources and Evaluation (LREC 2016). European Language Resources Association (ELRA), May 2016.

