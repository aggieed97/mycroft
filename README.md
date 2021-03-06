# Mycroft

Mycroft is a toolkit for doing text classification with word embeddings.

Out of the box Mycroft provides a command line interface for training and evaluating different kinds of neural network
classifiers.
It also provides a programmatic interface that help you write you own models. 
This interface allows you to focus on writing the model while getting data munging processing, text processing and
embedding, prediction and evaluation code which remains the same regardless of model, and a command line interface for
free.


## Installation

Install Mycroft by running `python setup.py install` in this directory.

Mycroft uses the [spaCy](https://spacy.io/) text natural language processing toolkit to process text.
The built-in models just use spaCy for tokenization, but models built on Mycroft's programmatic interface may use
spaCy's more advanced features like part-of-speech tagging and syntactic parsing.
By default it installs spaCy's English language text model, though you may specify other language models from the
command line.
See spaCy's [models documentation](https://spacy.io/docs/usage/models) for more information.


## Running from the Command Line

Run Mycroft with the command `mycroft`.
Subcommands enable you to train models and use them to make predictions on unlabeled data sets.
Run `mycroft --help` for details about specific commands.

The training data is a comma- or tab-delimited file with column of text and a column of labels.
The test data is in the same format without the labels.

Mycroft implements two kinds of word-embedding models.

* __Recurrent neural network__

  300-dimensional [GloVe](https://nlp.stanford.edu/projects/glove/) vectors are used to embed the text into matrices of
  size _sequence length × 300_, clipping or padding the first dimension for each individual text as needed.
  A stack of recurrent neural networks (either GRUs or LSTMs) converts these embeddings to a single vector which a
  softmax layer then uses to make a label prediction.

* __Convolutional network__

  This works the same as the recurrent neural network, except that it summarizes the sentence embedding matrices with
  a 1-dimensional convolutional/max-pooling network instead of a stack of recurrent neural networks. 

* __Bag of words__

  The same GloVe vectors are used to embed the tokens in the text.
  A softmax layer uses the average of the token embeddings to make a label prediction.

The hyper-parameters of these models are specified by command line parameters.
Command line parameters can also be passed in as a text file, one parameter per line, with the text file name prefixed
with an @ sign, e.g. `mycroft @my-args`. 

Evaluation on training and validation sets returns the classification accuracy and the cross entropy loss.

Run `mycroft demo` to see a quick example of the command line syntax and data formats.


## Programmatic Interface

You can write your own Keras-based text-embedding classifiers by extending the `mycroft.model.TextEmbeddingClassifier`
base class in and using subclasses of `mycroft.text.Embedder` to handle text processing and word embedding.

See `convolution_net.py` in the `examples` for detailed instructions on how to do this.
