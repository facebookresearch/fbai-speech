This repo contains the necessary files to replicate the LibriSpeech results in:

[1] Le et al. "Contextualized Streaming End-to-End Speech Recognition with Trie-Based Deep Biasing and Shallow Fusion." INTERSPEECH, 2021 (in submission).

## Computing WER, U-WER, and B-WER

The reference files can be found under the [ref](ref) folder in the following format: `test-{clean,other}.biasing_{N}.tsv`, where `N` is the size of the biasing list. Each row in the reference file contains 4 tab-separated columns: utterance ID, reference text, list of rare words in the reference text, and list of biasing words (i.e., distractors) which is guaranteed to be a superset of the previous rare word list.

The hypothesis file is expected to contain two tab-separated columns: utterance ID and hypothesis text. Example hypothesis files can be found under the [hyp](hyp) folder in the following format: `test-{clean,other}.{id}.*`, where `id` corresponds to the model ID described in [1].

Run the scoring script to compute WER, U-WER, and B-WER: (see [1] for definitions of these metrics)

```
python score.py --refs <ref_file> --hyps <hyp_file>
```

Example result files can be found under the [results](results) folder. These correspond to the numbers reported in [1].

## Word Lists

Under the [words](words) folder, you can find several relevant supporting files to replicate the training setup of [1]:

* `all_words.count.txt`: list of all words in the LibriSpeech training data (both audio and text-only), along with their occurrence counts in the audio training set. These words are sorted in descending occurrence count.
* `common_words_5k.txt`: list of all common words, defined as the first 5,000 words in `all_words.count.txt`.
* `all_rare_words.txt`: list of all rare words, defined as the difference between `all_words.count.txt` and `common_words_5k.txt`.

During training, `common_words_5k.txt` is used to identify rare words in the reference text and `all_rare_words.txt` is used to construct artificial biasing lists on-the-fly.
