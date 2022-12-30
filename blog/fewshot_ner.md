---
layout: page
title: few-shot named entity recognition
---

##### intro

practical named entity recognition often demands quick adaptation to new entity types and/or further languages. 
thus, the requirements to possible solutions are three-fold: (1) being extendable to new entities, (2) being
transferable to new languages and (3) being sample-efficient if labeled data is available. in the following, we will
look at the most prominent approaches how to overcome few-shot named entity recognition.

##### few-shot learning and sequence tagging

in sequence tagging, given some textual sequence $$ \mathbf{X} = (x_1, \dots, x_n) $$, we want to assign each token $$x_i$$
in the sequence its corresponding entity type $$ \mathbf{Y} = (y_1, \dots, y_n) $$. given a labeled dataset with $$ N $$
examples, we can optimize well-known neural networks using cross-entropy loss and backpropagation:
\$$ \ell(y, \hat{y}) = - \frac{1}{N} \sum_{n=1}^{N} H(y_n, \hat{y_n}) $$

with $$ H(p, q) $$ being the cross-entropy term.

if the labeled dataset has a sufficient size, this approach works well. however, there is a large gap in terms of performance
if there are only a few labeled data points available. this leads to the question, how to train models with a training 
dataset $$ D = \{\mathbf{X}_n, \mathbf{Y_n}\}_{n=1}^{N} $$ where $$ N $$ is small and still achieve a good generalization performance.

in the following we will look at the most prominent approaches that tackle the problem of few-shot named entity recognition.
[Huang et al.](https://arxiv.org/pdf/2012.14978.pdf) provide an excellent and more detailed view about the approaches.

##### prototype methods

prototype methods use episodic training to simulate few-shot settings already during model training. episodic training means that
one samples $$ M $$ entity types from $$ D $$ where $$ M < |C| $$ with $$ C $$ being the number of entity types.
Further, each episode has splits: a key, query and support set where each set contains $$ K $$ sentences per sampled entity type.

the other important part are prototypes. these are entity type representations in the same vector space of individual tokens.
a way to construct such a prototype representation $$ \mathbf{c}_m $$ is to take the average of all token representations
belonging to this entity type in the support set $$ S $$:

$$ \mathbf{c}_m = \frac{1}{|S_m|} \sum_{x \in S_m}^{} f_{\theta}(x) $$

with $$ f_{\theta} $$ being an embedding function.
to compute our loss, we calculate some distance between a token representation and the prototype representation, for instance
$$ d(f_{\theta}(x), \mathbf{c}_m) = || f_{\theta}(x) - \mathbf{c}_m ||_2 $$. finally, classification is performed by assigning
the nearest prototype as the label to the token.

##### noisy supervised pretraining

another simple but effective approach is noisy supervised pretraining. self-supervised pre-trained language models give us
already good representations. however, these models treat all tokens equally instead of highlighting certain token types which
is in turn the task of named entity recognition. instead of relying on well-crafted datasets, we can pretrain language models
once more using large-scale web data such as WiNER. This dataset is annotated automatically with anchored strings and their
entity mentions in their respective wikipedia articles. Even though, automatic annotation 

##### self-training

another semi-supervised approach is using a student-teacher architecture. for this approach, one needs labeled data $$ D^L $$ for 
training a trainer model that will generate pseudo labels for unlabeled data $$ D^U $$. the idea build upon previous terms:

1) a teacher model is trained using cross-entropy loss based on annotated examples.
2) this model generates pseudo labels for unlabeled data: $$ \mathbf{y}_i = f_{\theta^{tea}}(x_i)$$.
3) train a student model via cross-entropy on both labeled and pseudo labeled data:

$$ \ell_{stu} = \frac{1}{|D^L|} \sum_{x_i \in D^L}^{} \ell(f_{\theta^{stu}}(x_i), y_i) + \frac{1}{|D^U|} \sum_{x_i \in D^U}^{} \ell(f_{\theta^{stu}}(x_i), y_i) $$