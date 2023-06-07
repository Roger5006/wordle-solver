
# introduction

This solver will work in two different ways -- either the secret word may be
known to the user and specified to the program (useful for testing purposes, so
that the program can whittle down the search space until it finds the specified
word), or the solver can iteratively suggest guesses to the user, so as to
search for solutions to a live problem.

## picking a word to guess

There has been a lot of discussion on how to find the *optimal* guess words in
wordle (for example, see the [excellent](https://youtu.be/v68zYyaEmEA)
[videos](https://youtu.be/fRed0Xmc2Wg) from educational math YouTuber
3Blue1Brown) but here we will use a simple heuristic score -- we pick the
word that contains the letters that occur most commonly in the remaining
possible vocabulary.

So at each step in our solver, we select the word that maximizes the following
expression:

```
  score(word | vocabulary) = sum(score(letter | vocabulary), for unique letters in word)
```

where

```
  score(letter | vocabulary) = # words in vocabulary that contain letter
```

For our purposes here, this is *easier to understand* and *easier to implement*
than a more principled information-theoretic approach. Notably, the score
for a letter and a word both depend on the words that are currently in the
vocabulary, which we update based on the response received after every guess.

Relatedly, why do we care about only considering *unique* letters? Well, if we
do not, we will only get information about the most common letters. If we do not
include this restriction, our first guess (given the initial vocabulary) will be
"eerie", since 'e' is so common. But the answer for the word "eerie" is only so
informative -- we could do better by including a more diverse selection of
common letters! (you could [formalize this by quantifying how many bits of
information you will learn by asking the
question](https://en.wikipedia.org/wiki/Entropy_(information_theory)), but this
is more of a topic for an AI class)

Empirically, this heuristic works fairly well, and paired with the approaches to
whittling the vocabulary described in the next section, each guess causes the
size of our search space to *drastic* go down.

##Compiling and Running the Program

make

./solver


