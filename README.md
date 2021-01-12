# nlp
Natural Language Processing

## Overview
1. Parse English sentences into all possible sentence diagram objects.
2. Use a trainable Markov-esque association to weight diagram objects and rank from most to least likely.
3. Use a Convolutional Neural Net to check and correct the associations of the map.

## Steps
1. Download a dictionary object of English words complete with parts of speech and a differentiation between multiple definitions.
2. Create a sentence diagram object that tracks subjects, verbs, and modifiers
3. Create a program grammar to allow sentences to tokenize
4. Create algorithm to tokenize sentences into grammar.
5. Build a tiered Markov graph which uses in-sentence context and circum-sentence context to weight tokens.
6. Build a CNN to adjust the Markov weights until the previous CNN selects it.

### Potential future work
1. CNN or Markov graph to look for synonyms.
2. CNN or Markov graph to do indirections related to pronouns or articles

### Probable Implementation
1. SQL dictionary search, indexed on text
2. Rust or Haskell grammar definition, Rust preferred
3. Rust algorithm for generating diagram objects

### Potential algorithm for parsing
1. Collect all words in sentence
2. Fetch all words from dictionary matching the literal text
3. Create a multiplied set for all versions of the words
4. For each set:

  Attempt to parse the text into the grammar
  
  If the parse fails due to mismatch on part of speech, discard the parse.
  
  If all parses fail, throw an error. Either an error occurred or the sentence was not a valid English sentence
  
  Return all successful parses

### Markov association design
Markov association will consist of multiple "tiers", which will be simplified by summed Bayesian probability.

The first tier will consist of raw probabilities. This level of probability is likely the least useful, but will largely discard arcane or archaic uses of the word.

The second tier will consist of direct-modifier-based probabilities. This step will likely be the most useful. This map will contain all known noun-adjective, verb-adverb, and adjective-adverb associations.

The third tier will consist of in-sentence associations. This map will associate the word with all other words in the sentence. This map may grow absurdly large and will probably require extensive pruning.

The fourth tier will consist of circum-sentence associations. This map will associate nouns and verbs in the sentence with nouns and verbs in the surrounding sentences in the same paragraph.

The final tier will consiste of circum-speaker associations. This map cannot be used unless it is parsing a conversation between two speakers. This map will associate nouns and verbs from all paragraphs from one speaker with nouns and verbs from all paragraphs of the other speaker.

It is unlikely that all these tiers will be necessary or built, but the predicted order of usefulness will be: 2, 3, 1, 4, 5 and tiers will likely be constructed in that order if there are time constraints.

### Adjuster CNN design

While the system might be able to funciton off rote probability feedback to catalog its experiences, having a CNN to adjust the weights of the Markov map to shift the probabilities so that the correct mappings will rise to the top.

The two options are adjustments on the raw probabilities or adjustments on the "tiers".
