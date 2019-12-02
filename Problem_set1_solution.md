## Problem 1 - Positional Indexes (5 points)

a.
First, let's build the positional index for the word "I":

"I" appears in Doc 1 at positions 1 and 7, and in Doc 2 at position 1. So the positional index for "I" would be:
I: 1: <1, 7>; 2: <1>

Next, let's build the positional index for the word "CS276":

"CS276" appears in Doc 1 at position 5, and in Doc 2 at position 3. So the positional index for "CS276" would be:
CS276: 1: <5>; 2: <3>

Therefore, the positional indexes for the words "I" and "CS276" are as follows:

I: 1: <1, 7>; 2: <1>
CS276: 1: <5>; 2: <3>

Each line represents a document ID followed by the positions where the corresponding word appears in that document.


b.
i. "I student":
We need to find occurrences where "I" is immediately followed by "student". According to the snippet, a phrase query retrieves occurrences where word1 is immediately followed by word2. From the snippet, we don't have information about the positions of the words "I" and "student" in the documents. Therefore, we cannot determine if the query conditions are met. We would return None for this query.

ii. "student I":
Similar to the previous query, we need to find occurrences where "student" is immediately followed by "I". Again, we don't have information about the positions of the words "student" and "I" in the documents. Therefore, we cannot determine if the query conditions are met. We would return None for this query as well.

iii. "I /2 student":
For this query, we need to find occurrences of "I" within 2 words of "student" on either side. Unfortunately, the snippet does not provide the necessary positional information to determine if the query conditions are met. Therefore, we would return None for this query.

iv. "student /2 I":
Similar to the previous query, we need to find occurrences of "student" within 2 words of "I" on either side. Since the snippet does not provide the positional information, we cannot determine if the query conditions are met. We would return None for this query.

In summary, based on the provided search result snippet, we cannot determine the positions of the words in the documents, so we cannot accurately evaluate the given queries. Therefore, the result for all the queries would be None.

c.
i. To modify the positional index to support queries that demand terms to be in the same sentence, we can add an additional level of indexing that represents the sentences in a document. This can be achieved by extending the positional index structure to include sentence information.

The modified positional index structure would look like:
Word1: DocID1: Sentence1: <Positionabc, Positionxyz>; Sentence2: <Positiondef, Positionghi>; ...
Word2: DocID1: Sentence1: <Positionjkl>; Sentence2: <Positionmno>; ...

This structure allows us to retain the existing document and position information while introducing the concept of sentences. Each word's postings list is expanded to include sentences, and each sentence entry contains the positions of the word within that sentence.

ii. Example of the modified postings list for the words "I" and "CS276":

"I": 
Doc1: 
  Sentence1: <1, 7>
  Sentence2: <3>
Doc2: 
  Sentence1: <1>

"CS276": 
Doc1: 
  Sentence1: <5>
Doc2: 
  Sentence1: <3>

In this example, we have two documents (Doc1 and Doc2). The word "I" appears in Doc1 at positions 1 and 7, in Sentence 1, and at position 3 in Sentence 2. In Doc2, "I" appears at position 1 in Sentence 1. The word "CS276" appears in Doc1 at position 5 in Sentence 1 and in Doc2 at position 3 in Sentence 1.

By incorporating sentence information into the positional index, we can now perform queries that demand terms to be in the same sentence. This modification allows us to more accurately retrieve documents where the terms "I" and "CS276" are at most 3 words apart within the same sentence.

## Problem 2 - Dictionary Storage Compression (10 points)

a.
 Assume the dictionary data structure uses dictionary-as-a-string storage with front coding.

i. Block size of 3:
To determine the resulting storage, we need to sort the terms in the vocabulary and apply front coding. Let's arrange the terms in alphabetical order:

1. eligible
2. elongate
3. eloquent
4. elope
5. ellipse
6. elite

Using front coding, we encode the common prefix between consecutive terms with a special symbol ♦. The resulting storage for a block size of 3 would be:

eligible
elongate
eloquent
♦pe
♦se
♦ite

ii. Block size of 6:
Again, let's sort the terms in alphabetical order:

1. eligible
2. elongate
3. eloquent
4. elope
5. ellipse
6. elite

Using front coding with a block size of 6, the resulting storage would be:

eligible
elongate
eloquent
elope
ellipse
elite

Please note that the resulting storage may vary depending on the specific implementation of front coding and the chosen block size.

b.
To calculate the total dictionary storage required for the given vocabulary, we will consider two scenarios:

i. Fixed width entries of 20 bytes:
Since each character occupies 1 byte, and each term is 8 characters long on average, the storage required for each term would be 8 bytes. Additionally, we have a 4-byte document frequency and a 4-byte pointer to the postings list. Therefore, the total storage required for each term would be 8 + 4 + 4 = 16 bytes.

Given that the vocabulary consists of 6 terms, the total dictionary storage required would be 6 terms * 16 bytes/term = 96 bytes.

ii. Blocked storage with front coding and a block size of 3:
In this case, each term pointer occupies 3 bytes. The vocabulary storage is determined by the front coding compression method.

From the previous answer in part (a) (i), we have the resulting storage for a block size of 3 as follows:
eligible
elongate
eloquent
♦pe
♦se
♦ite

Considering that each character occupies 1 byte, we can calculate the total storage required for the vocabulary. The terms "eligible," "elongate," and "eloquent" each require 8 bytes. The front coding symbols "♦pe," "♦se," and "♦ite" each require 4 bytes. Therefore, the total storage required would be 3 terms * 8 bytes/term + 3 symbols * 4 bytes/symbol = 24 + 12 = 36 bytes.

To calculate the proportion by which we reduce storage using blocked storage with front coding and a block size of 3, we can compare the storage requirements between the two scenarios:

Storage reduction proportion = (Storage without front coding - Storage with front coding) / Storage without front coding * 100%

Using the values calculated above:
Storage reduction proportion = (96 bytes - 36 bytes) / 96 bytes * 100% = 60%

Therefore, using blocked storage with front coding and a block size of 3 reduces the storage requirement by 60%.

c. 

To determine the maximum size of the vocabulary that can be resolved using 3-byte term pointers, we need to consider the average length of each vocabulary entry and the compression achieved by the front coding method.

Given that each vocabulary entry is 8 characters long on average, and the front coding compression method compresses the dictionary string by 20%, we can calculate the maximum size of the vocabulary.

The compressed size of each vocabulary entry would be 8 characters * (1 - 0.20) = 6.4 characters on average.

Since each character can be stored in 1 byte, the maximum size of the vocabulary that can be resolved using 3-byte term pointers would be 3 bytes / 6.4 characters = 0.46875 bytes/character.

Therefore, the maximum size of the vocabulary that can be resolved using 3-byte term pointers is approximately 0.46875 bytes/character.

Please note that these calculations are based on the given assumptions and may vary depending on the specific implementation and compression techniques used.


