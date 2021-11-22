# Undergrad-protein-pretrain

**A talk targeted at junior undergraduates organized by the KEG group**

Materials for `Protein Pre-training Model`



## Background
Protein is related to almost every life process. Therefore, analyzing the biological structure and characteristics of protein sequences is essential for exploring life, disease detection, and drug discovery. Traditional protein analysis methods are often labor-intensive and time-consuming. The emergence of deep learning models makes it possible to model data patterns in large amounts of data. Interdisciplinary researchers have begun to use deep learning methods to model large biological data sets, such as the use of long and short-term memory and convolutional neural networks for protein sequence classification. After millions of years of evolution, evolutionary information is encoded in protein sequences. Large-scale pre-trained language models can be used to extract these types of information.


## Releted Works
### TAPE
- Paper [Evaluating Protein Transfer Learning with TAPE](https://arxiv.org/abs/1906.08230)
- Code [TAPE](https://github.com/songlab-cal/tape)

### ProteinLM
- Paper [Modeling Protein Using Large-scale Pretrain Language Model](https://arxiv.org/abs/2108.07435)
- Code [ProteinLM](https://github.com/THUDM/ProteinLM)


### ProtTrans
- Paper [ProtTrans: cracking the language of life’s code through self-supervised deep learning and high performance computing](https://arxiv.org/abs/2007.06225)
- Code [ProtTrans](https://github.com/agemagician/ProtTrans)


### ESM
- Paper [Biological structure and function emerge from scaling unsupervised learning to 250 million protein sequences](https://www.biorxiv.org/content/10.1101/622803v4)
- Code [ESM](https://github.com/facebookresearch/esm)



## Pretrain Process
Just like population evolution, active macromolecules (such as proteins) in organisms have also undergone heredity and mutation during the evolution process. After millions of years of natural selection, amino acid sequences that exist stably in nature have specific distribution patterns, and most of them are in a low-energy stable state. The distribution of amino acids in protein sequences is controlled by biological grammar, which can be captured using language models.

We can use MLM (masked language model) to capture these biological "grammar".

### Dataset

Use publicly available protein primary sequence databases, such as PFAM (protein family), UniRef, etc.

### Model
Protein pretrain models adopt the architecture of the BERT model, one minor difference is that the input of the model is a single protein sequence, and the token type embedding is removed. In other words, the data in the model will go through the embedding layer (word embedding + position embedding), N * transformer layers (self-attention layer + FFN layer), and the LM-head of the output layer.

### Pretrain Objective
$$
\mathcal{L}_{\mathrm{MLM}}=-\sum_{\hat{x} \in m(\mathbf{x})} \log p\left(\hat{x} \mid \mathbf{X}_{\backslash m(\mathbf{x})}\right)
$$


### Fine-tune
When the pre-training model converges (reflected by mlm loss or ppl), we can fine-tune the model on downstream tasks.
The fine-tuning of downstream tasks (such as secondary structure prediction, homology detection, thermal stability, fluorescence, contact prediction, etc.) requires a small amount of labelled data. In fine-tuning, the pre-training language model acts as an encoder. When we perform a full-sequence task, we generally use the representation corresponding to the CLS token to perform classification or regression prediction. When we perform a token-level prediction task, we usually take representation for each token for prediction. When we perform the token-pair prediction task, we can concatenate the representations corresponding to the two tokens to do downstream task prediction.

