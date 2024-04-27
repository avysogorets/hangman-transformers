## Hangman
In Hangman, you need to guess a word where all of its letters are hidden behind masks. In each turn, you name a letter and,
if it appears in the word, all instances of it are unmasked. If it does not, you incur a penalty loss. You lose the game if
your penalty reaches 6 before you guess the word and win otherwise.

### Strategy
The key to playing Hangman, to a large extent, lies in extracting patterns of natural language such as word structure and grammar. 
Thus, we adopt the NLP framework and attempt to solve the task with a BERT-like transformer. The vocabulary will consist of 26 
English letters plus two special tokens: mask and padding; the output layer is a classification head with 26 outputs. The transformer
acrhitecture is suitabe for the task as it can naturally learn to focus on some local patterns within words via the attention mechanism.
In fact, it is common to train LLMs using "masked language modeling", which is similar to the given problem. The proposed procedure
is two-stage:
- ***Stage 1:*** we first pre-train a transformer and teach it to identify patterns in words. To this end, akin to masked language 
modeling, we mask *one* random letter in each word with the goal of predicting it.
- ***Stage 2:*** since the model will often be presented with more than one blanks in one word, we need to finetune it on the distribution 
of inputs as close to the real game scenario as possible. To this end, for each training word, we randomly mask out all instances of some
 subcollection of its letters and perform a multi-label classification with the goal of predicting all of those masked letters.
