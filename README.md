[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/waveletdeboshir/whisper-lang-remover/blob/main/whisper_lang_remover.ipynb)

# Whisper Language Remover

Prune tokenizer vocab to remove languages from Whisper.

Method from [this post](https://medium.com/m/global-identity-2?redirectUrl=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-adapt-a-multilingual-t5-model-for-a-single-language-b9f94f3d9c90) by David Dale was used.

In this method we will:
* choose tokens that we want to keep
* trim and renumber weights in whisper decoder embedding layer and last linear layer
* change whisper tokenizer
* save trimmed model.

# 🤗 HF Models
I published several models for russian on huggingface. 10% of tokenizer vocab was left including special whisper tokens (no language tokens except <|ru|> and <|en|>, no timestamp tokens), 200 most popular tokens from tokenizer and 4000 most popular Russian tokens computed by tokenization of russian text corpus.

| model | original whisper size | pruned vocab whisper size |
| :------ | :------ | :------ |
|[waveletdeboshir/whisper-small-ru-pruned](https://huggingface.co/waveletdeboshir/whisper-small-ru-pruned)| 242 M | 205 M |
|[waveletdeboshir/whisper-base-ru-pruned](https://huggingface.co/waveletdeboshir/whisper-base-ru-pruned)| 74 M | 48 M |
|[waveletdeboshir/whisper-tiny-ru-pruned](https://huggingface.co/waveletdeboshir/whisper-tiny-ru-pruned)| 38 M | 19.4 M |

finetuned versions:
* https://huggingface.co/waveletdeboshir/whisper-base-ru-pruned-ft
* https://huggingface.co/waveletdeboshir/whisper-small-ru-pruned-ft

# About Whisper medium and large
 This method will not be very effective for large whisper models. If we calculate the ratio of the number of parameters of the embedding layer in decoder and the last linear layer to the total number of parameters of the model, we will understand this:

| model | proportion of token embeddings and proj_out layers|
| ---- | ---- |
| tiny | 0.6906 |
| base | 0.5357 |
| small | 0.2829 |
| ---- | ---- |
| medium | **0.1300** |
| large-v3 | **0.0825** |

This ratio for medium and large models is small, so we will not see significant model size reduction.