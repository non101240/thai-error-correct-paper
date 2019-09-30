# Notes

## Task 01: Re-evaluate the Thai Error correction results with WER

Status: In-progress

Notes:
Summary
1. Rerun evaluation routine which will create a copy of the "corrected resulting text"
2. Verify that the results are consistent to the old results
3. Run the WER code in `notebook/scripts/018-wer-evaluation/WER.ipynb` notebook
4. Update GLEU results in Paper, GDrive
5. Added WER results in Paper

We are using WER code snippet from https://web.archive.org/web/20171215025927/http://progfruits.blogspot.com/2014/02/word-error-rate-wer-and-word.html

Note as we no longer have the BiGRU model we will have to retrain and re-evaluate that (TODO)

### Sub-task 01: Map the results in the Paper back to the Google Drive sheet

Status: Done

Notes:
The current results are from
Google Sheets [Results as of 2019-06-12](https://docs.google.com/spreadsheets/d/1y6b5fllDCEE_twkQTN7nRfREhO94yjEzjmXyUe9co7g/edit#gid=730642114) **"Correction" Sheet at the top**. Except for Hunspell, Hunsepll (re-trained), and Copy-Augmented Transformer.

**Hunspell** results are on the **"Dictionary Correction Performance" sheet**. With the best train dictionary using words that it has seen "1" time(s) for it to detect as a non-error. Note that both dicts are worse than "Do nothing".

**Copy-Augmented Transformer** results are on **"Correction" Sheet at the lower right** for "Copy-Augmented x0.75". As this is a fine tuned model to fit with our smaller dataset.

### Sub-task 02: Map the results from the Google Drive sheet to the results in the computer

Status: Done

Notes:
Results for 
* Do nothing
  * `experiments-result/004-getting-publication-again/correction/101-nothing/cunlp-v2-error/infer/on-test.report.json`
* Oracle
  * `experiments-result/004-getting-publication-again/correction/100-oracle/cunlp-v2-error/infer/on-test.report.json`
* Hunspell (Off the shelf)
  * `notebook/scripts/008-trainable-model-for-thai-error-correction/default/correction/on-test.report.json`
* PyThaiNLP
  * `experiments-result/004-getting-publication-again/correction/200-pythainlp/cunlp-v2-error/infer/on-test.report.json`
* BiGRU
  * **DOES NOT EXIST** The numbers are from old results
* Copy-Augmented Transformer
  * `text-cor-explore` `fairseq_gec/my/out/models/006-larger-than-003-model-retraining/infer/gleu-results.txt`
* Hunspell (Trained)
  * `notebook/scripts/008-trainable-model-for-thai-error-correction/train-th1/correction/on-test.report.json`
* Ours
  * `experiments-result/004-getting-publication-again/correction/001-correction-hybrid/infer/on-test-t0.5.report.json`
* Ours (SentencePiece)
  * `experiments-result/004-getting-publication-again/correction/002-correction-hybrid-sentencepiece/infer/on-test-t0.5.report.json`
* Ours (Oracle Detection)
  * `experiments-result/004-getting-publication-again/correction/001-correction-hybrid/infer/on-oracle-test.report.json`
* Ours (SentencePiece + Oracle Detection)
  * `experiments-result/004-getting-publication-again/correction/002-correction-hybrid-sentencepiece/infer/on-oracle-test.report.json`

*Finding Hunspell*

Found Hunspell Detection at `experiments-result/004-getting-publication-again/detect/005-hunspell`. 
Found Hunsepll experiment in this notebook
`notebooks/notebook/scripts/008-trainable-model-for-thai-error-correction/008-trainable-model-for-thai-error-correction.ipynb`

*Finding PyThaiNLP*

Found Evaluation script at `experiments/004-getting-publication-again/scripts/correction/201-pythainlp.fish`

*Copy-Augmented Transformer*

Found very useful readme at `text-cor-explore` `fairseq_gec/my/reports/on-cunlp.md`

### Sub-task 03: Re-learn how to run our current results

Status: Done

Notes:
We backed up all the reports file `*.report.json` to `*.report.json.bak` to ensure consistency.

To produce the inference files for models under `experiments-result/004-getting-publication-again/correction`,
the evaluation script was modified and re-run. The evaluation script is at
`experiments/004-getting-publication-again/scripts/correction/100-evaluation.fish`.

For Hunspell models the existing evaluation script already produces the `.cor` file. The existing evaluation script
is at `notebook/scripts/008-trainable-model-for-thai-error-correction/001-evaluate.fish`. We just re-run the script
and the results are consistent.

Copy-Augmented Transformer, the file is already there. Re-runing the evalution shows consistent results.

The Inference results are the files bellow
* Do nothing
  * `experiments-result/004-getting-publication-again/correction/101-nothing/cunlp-v2-error/infer/on-test.cor`
* Oracle
  * `experiments-result/004-getting-publication-again/correction/100-oracle/cunlp-v2-error/infer/on-test.cor`
* Hunspell (Off the shelf)
  * `notebook/scripts/008-trainable-model-for-thai-error-correction/default/correction/on-test.cor`
* PyThaiNLP
  * `experiments-result/004-getting-publication-again/correction/200-pythainlp/cunlp-v2-error/infer/on-test.cor`
* BiGRU
  * **DOES NOT EXIST** The numbers are from old results
* Copy-Augmented Transformer
  * `text-cor-explore` `fairseq_gec/my/out/models/006-larger-than-003-model-retraining/infer/model_2_formatted_n_filled`
* Hunspell (Trained)
  * `notebook/scripts/008-trainable-model-for-thai-error-correction/train-th1/correction/on-test.cor`
* Ours
  * `experiments-result/004-getting-publication-again/correction/001-correction-hybrid/infer/on-test-t0.5.cor`
* Ours (SentencePiece)
  * `experiments-result/004-getting-publication-again/correction/002-correction-hybrid-sentencepiece/infer/on-test-t0.5.cor`
* Ours (Oracle Detection)
  * `experiments-result/004-getting-publication-again/correction/001-correction-hybrid/infer/on-oracle-test.cor`
* Ours (SentencePiece + Oracle Detection)
  * `experiments-result/004-getting-publication-again/correction/002-correction-hybrid-sentencepiece/infer/on-oracle-test.cor`

#### Sub-task 01: Verify that the new report on Thai Dataset is consistent with the old results

Status: Done

Verify these files
Compare the original evaluation to the re-run evaluations

Verified
* experiments-result/004-getting-publication-again/correction/101-nothing/cunlp-v2-error/infer/on-test.report.json
* experiments-result/004-getting-publication-again/correction/101-nothing/cunlp-v2-error/infer/on-test.report.json.bak

Verified (Note there are some minor differences that can be ignored)
* experiments-result/004-getting-publication-again/correction/100-oracle/cunlp-v2-error/infer/on-test.report.json
* experiments-result/004-getting-publication-again/correction/100-oracle/cunlp-v2-error/infer/on-test.report.json.bak

Verified
* notebook/scripts/008-trainable-model-for-thai-error-correction/default/correction/on-test.report.json
* notebook/scripts/008-trainable-model-for-thai-error-correction/default/correction/on-test.report.json.bak

Verified
* experiments-result/004-getting-publication-again/correction/200-pythainlp/cunlp-v2-error/infer/on-test.report.json
* experiments-result/004-getting-publication-again/correction/200-pythainlp/cunlp-v2-error/infer/on-test.report.json.bak

Verified
* notebook/scripts/008-trainable-model-for-thai-error-correction/train-th1/correction/on-test.report.json
* notebook/scripts/008-trainable-model-for-thai-error-correction/train-th1/correction/on-test.report.json.bak

Verified
* experiments-result/004-getting-publication-again/correction/001-correction-hybrid/infer/on-test-t0.5.report.json
* experiments-result/004-getting-publication-again/correction/001-correction-hybrid/infer/on-test-t0.5.report.json.bak

Verified (Note there are some minor differences that can be ignored, value is updated in paper and GDrive)
* experiments-result/004-getting-publication-again/correction/002-correction-hybrid-sentencepiece/infer/on-test-t0.5.report.json
* experiments-result/004-getting-publication-again/correction/002-correction-hybrid-sentencepiece/infer/on-test-t0.5.report.json.bak

Verified
* experiments-result/004-getting-publication-again/correction/001-correction-hybrid/infer/on-oracle-test.report.json
* experiments-result/004-getting-publication-again/correction/001-correction-hybrid/infer/on-oracle-test.report.json.bak

Verified (Note there are some minor differences that can be ignored, value is updated in paper and GDrive)
* experiments-result/004-getting-publication-again/correction/002-correction-hybrid-sentencepiece/infer/on-oracle-test.report.json
* experiments-result/004-getting-publication-again/correction/002-correction-hybrid-sentencepiece/infer/on-oracle-test.report.json.bak

~~The results for the sentencepiece models are from the incorrect tokenization.~~ The sentencepiece score are now consistent (mostly) with the old results.

#### Sub-task 02: Find the root cause of the sentence model not producing the correct evaluation in the rerun

Status: Done

There are 2 issues
1. Minor differences in the GLEU score, this is because of fixing bugs in the test set. the minor differences can be ignore as it does not change the overall conclusion
2. WER can not be compared directly because of the sentence-piece limitation of automatic text preprocessing which results in some " " (spaces) tokens being removed.
  * We are ignoring this for now.

### Sub-task 04: Evaluate inference using WER

Status: Done

WER code in `notebook/scripts/018-wer-evaluation/WER.ipynb` notebook

## Backlog: Trian sentence level error detection model for Thai UGWC

Status Pending

## Backlog: Fine-tune sentence and word level detection at same time (Conll)

Fine-tune word-level detection after sentence-level detection

Status: Pending

## Backlog: Re-evaluate everything with the sentence-piece like post-processing

Status: Pending

## Backlog: Fine-tune the detection threshold using the validation

Info: Currently, the "Ours (SentencePiece) 2-Stage 0.9326" is not the best score according to Google Sheet "Multi-Th SentencePiece" Sheet. As the results is the score from using "detection threshold" of "0.5". However, running against the test set shows that "0.6" is better. But, we cannot directly use that unless we verify that we can obtain the "0.6" from the validation set.