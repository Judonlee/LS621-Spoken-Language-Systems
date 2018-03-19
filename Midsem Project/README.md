# Project Report

### Data

* The task language is **Marathi** and the task consists of recognising a sequence of **7 digits**.
* **5sec** speech samples of **3 speakers** have been collected for experimentation.
* Speakers 1 and 2 have recorded a set of **40 prompts** provided in **digitSequences40b.txt** which serve as the training data.
* Additionally, each speaker has recorded a set of **20 prompts** which serve as the test data. These prompts were generated by using the **HSGen** tool provided in HTK.
* All speech files in *wav* format can be found in folder **wavMR_[0-2]** labelled **sample**(train) and **test** 

- - -

### Experiments

I have run **2 experiments** for the purpose of the project.

| Exp No. | Train set speaker id |
| :---: | :---: |
| 1 | 0 |
| 2 | 0,1 |

In both cases, word-level and sentence-level accuracies are reported.

- - -

### Steps

Desciption of all files can be found in **FILEINFO.txt**.

The file **run\_exp.sh** will run the 2 experiments mentioned above and generate the required HMMs in the folders exp1 and exp2 respectively. The steps involved in the training of the HMMs are clearly outlined in **run_exp.sh**.

The following files need to be modified for adapting the script for a new languages and tasks:

-	task_language/task_grammar
	Define the words available in the task and the allowed order. This generates the wordnet for the task which is required by other tools
	
-	task_language/mr_pronunciation_dict
	Defines the word to phone mapping for the language.

-	task_language/en2mr_dict
	Defines word mapping from the source language to target language (here Marathi) for transcribing prompts

- - - 

### Results

##### Notation: Accuracies (out of 100%) are reported. H := correct transcriptions, S := substitution errors, I := insertion errors, N := total samples

#### Exp1

| | Speaker 0 Acc | Speaker 1 Acc | Speaker 2 Acc |
| --- | --- | --- | --- |
| SENT | 90.00 [H=18, S=2, N=20] | 25.00 [H=5, S=15, N=20] | 45.00 [H=9, S=11, N=20] |
| WORD | 98.57 [H=138, D=0, S=2, I=0, N=140] | 80.00 [H=115, D=3, S=22, I=3, N=140] | 88.57 [H=125, D=1, S=14, I=1, N=140] |

#### Exp2

| | Speaker 0 Acc | Speaker 1 Acc | Speaker 2 Acc |
| --- | --- | --- | --- |
| SENT | 55.00 [H=11, S=9, N=20] | 70.00 [H=14, S=6, N=20] | 25.00 [H=5, S=15, N=20] |
| WORD | 91.43 [H=128, D=0, S=12, I=0, N=140] | 95.71 [H=134, D=0, S=6, I=0, N=140] | 83.57 [H=117, D=0, S=23, I=0, N=140] |

- - - 

### Analysis

1.	In **Exp 1** test samples of speaker 1 & 2 were both new to the model and accordingly sentence accuracy is lower than that for the observed speaker (0). In **Exp 2** test samples of speaker 2 were new to the model and accordingly sentence accuracy is lower than that for the observed speaker (0,1). In a sense, as expected, the HMM fits to the speaker available at training time and hence performs poorly for a new speaker.
2.	Accuracy for the training set speaker are better in **Exp 1** than in **Exp 2**. This is an expected behaviour as the second HMM has to fit a broader/more complex distribution.
3.	These trends can be observed slightly better in word-level accuracies. However, being exposed to 2 speakers does **not** improve the test time performance by a considerable margin. This may be attributed to scarcity of data.
4.	There are a few deletion and insertion errors which is surprising considering the standard nature of the task language (7 digit sequence). Although it is unclear in the documentation, this may be the accuracy for phone level transcription in which case the deletions and insertions make sense.
5.	There are a few recurring substitutions in the transcriptions. E.g. NAUU -> DON. This example is especially surprising considering the different phonetic structure.
6.	Although speaker 2 samples were recorded in a different environment (time of day, place) than those for speaker 0 and 1, this should not be considered as justification the HMMs poor performance considering that the end user environment is expected to be different from the training environment.