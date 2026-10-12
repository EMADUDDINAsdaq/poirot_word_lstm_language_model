### Introduction
This project evaluates style-constrained text generation using a word-level LSTM model trained on a single Agatha Christie novel. Word-level modelling was required and selected to preserve lexical style and dialogue structure, despite increased sparsity relative to character-level approaches. The objective was to assess the model's ability to generate stylistically similar detective fiction from a given seed phrase while avoiding memorisation. Performance was evaluated using accuracy, perplexity, and 4-gram overlap, and the generated output was compared with a transformer-based large language model to highlight architectural differences.

### Data Preprocessing

Project Gutenberg metadata and non-narrative sections were removed to ensure the model learned only from the author's prose. The text was cleaned through punctuation spacing and normalisation, then tokenised using Keras TextVectorization, enabling distributed word representations [2]. Vocabulary analysis showed that 95% of usage was covered by the top 5000 words (from ~7000 total), so the vocabulary was capped at 5000 to reduce sparsity and computational complexity while maintaining stylistic richness. A sequence length of 100 was selected to provide adequate contextual information. The dataset was split 80/20 for training and testing, with the training portion further divided 90/10 for validation.

### Model and Training

The architecture consisted of a 256-dimensional embedding layer followed by two stacked LSTM layers [1] to improve sequential modelling and contextual representation. Dropout was increased from 0.3 to 0.4 to enhance stability and reduce overfitting, and L2 regularisation was applied to the embedding layer. The model was trained using the Adam optimiser with early stopping and learning-rate reduction to stabilise convergence. Early stopping and dropout were explicitly used to improve generalisation. Validation loss plateaued after approximately ten epochs, indicating limited further gains from additional training.

### Evaluation and Comparison

Although overall accuracy remained modest, perplexity—a standard evaluation metric in language modelling [3]—improved significantly (from 500 to 300) after vocabulary capping and hyperparameter tuning, indicating improved next-word prediction stability. A low 4-gram overlap (~0.02) suggests that the model generates novel text rather than memorising passages. Qualitatively, the LSTM captures local stylistic features such as dialogue structure and detective vocabulary, but exhibits grammatical inconsistencies and weak long-range coherence. In contrast, transformer-based models rely on self-attention mechanisms to model global context more effectively [4], producing grammatically coherent and logically structured output. The LLM benefits from large-scale pretraining and attention-based contextual modelling, whereas the LSTM compresses context sequentially through hidden states. This comparison highlights the trade-off between model simplicity and expressive power, demonstrating both the capability and architectural limitations of word-level LSTMs in style-constrained generation.

### References

[1] S. Hochreiter and J. Schmidhuber, “Long short-term memory,” Neural Computation, vol. 9, no. 8, pp. 1735–1780, 1997.

[2] T. Mikolov, I. Sutskever, K. Chen, G. Corrado and J. Dean, “Distributed representations of words and phrases and their compositionality,” in Advances in Neural Information Processing Systems (NeurIPS), 2013.

[3] S. F. Chen and J. Goodman, “An empirical study of smoothing techniques for language modeling,” in Proc. ACL, 1996, pp. 310–318.

[4] A. Vaswani et al., “Attention Is All You Need,” in Advances in Neural Information Processing Systems (NeurIPS), 2017.

###Output from LLM(ChatGPT)
For starting seed (input) : "Poirot looked at me gravely and said"

", with that peculiar deliberation which always heralded some disquieting revelation. “My friend,” he murmured, adjusting the crease of his immaculate cuff, “you observe—but you do not perceive. That is the tragedy of the honest mind.”

I protested that I had perceived a great deal, but he silenced me with a gentle uplift of his hand.

“The facts, they arrange themselves with admirable symmetry. It is the human heart that introduces disorder. Consider the teacup—untouched. The window—latched from within. And yet—” He inclined his head. “There has been design. A design most subtle.”

A chill crept over me. “You suspect murder?”

Poirot sighed softly. “I suspect intelligence, Hastings. And that, perhaps, is far more dangerous.”
