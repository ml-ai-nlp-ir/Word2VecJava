# Word2vecJava

This is a port of the open source C implementation of word2vec (https://code.google.com/p/word2vec/)

Note that this isn't a completely faithful rewrite, specifically:

### When building the vocabulary from the training file:
1 .The original version does a reduction step when learning the vocabulary from the file when the vocab size hits 21 million words, removing any words that do not meet the minimum frequency threshold. This Java port has no such reduction step.
2. The original version injects a </s> token into the vocabulary (with a word count of 0) as a substitute for newlines in the input file. This Java port's vocabulary excludes the token.

### In partitioning the file for processing
1. The original version assumes that sentences are delimited by newline characters and injects a sentence boundary per 1000 non-filtered tokens, i.e. valid token by the vocabulary and not removed by the randomized sampling process. Java port mimics this behavior for now ...
2. When the original version encounters an empty line in the input file, it re-processes the first word of the last non-empty line with a sentence length of 0 and updates the random value. Java port omits this behavior.

### In the sampling function
1. The original C documentation indicates that the range should be between 0 and 1e-5, but the default value is 1e-3. This Java port retains that confusing information.
2. The random value generated for comparison to determine if a token should be filtered uses a float. This Java port uses double precision for twice the fun

### In the distance function to find the nearest matches to a target query
1. The original version includes an unnecessary normalization of the vector for the input query which may lead to tiny inaccuracies. This Java port foregoes this superfluous operation.
2. The original version has an O(n * k) algorithm for finding top matches and is hardcoded to 40 matches. This Java port uses Google's lovely com.google.common.collect.Ordering.greatestOf(java.util.Iterator, int) which is O(n + k log k) and takes in arbitrary k.

Note: The k-means clustering option is excluded in the Java port

Please do not hesitate to peek at the source code. It should be readable, concise, and correct. Please feel free to reach out if it is not.

## Building the Project
To verify that the project is building correctly, run 
```bash
./gradlew build && ./gradlew test
```

It should run 7 tests without any error.


