## 1. Accessing Text Corpora

### 1.1 GutenBerg Corpus
gutenberg project := 25000 text book archive \
nltk.corpus.gutenberg.fileids() <- why not field????? \
raw() -> all letters in the text \
sents() -> all sentence in the text

### 1.2 Web and Chat Text
NLTK has small collection of web text. \
Web forum, conversations, movie script, etc
* Instant messaging chat sessions also exist

### 1.3 Brown corpus
![Image of Table](./Table1.png)

Brown Corpus was the first million-word electronic corpus of Eng.\
It contains text from 500 sources, categorized by genre. \
Brown Corpus is convenient resource for studying systematic diff btw genres.

### 1.4 Reuters Corpus 
10,788 news documents totaling 1.3m words \
classified into 90 topics, grouped into two sets `training`, `test` \
Unlike Brown corpus, categories in the Reuters corpus overlap with each other

### 1.5 Inaugural Address Corpus(대선 연설)
Collection of 55 texts, one each presidential address. \
It has time demension. Year of each text appears in its filename 
![inaugural address dist](./fig1_1.png)

### 1.8 Text Corpus Structure
![structure](./fig1_3.png)
Occasionally, text collections have temporal structure. \
Above categories are most common example.

## 2. Conditional Frequency Distributions
### 2.3 Plotting and Tabulating Distributions

ConditionalFreqDist = Important!

languages = ['Chickasaw', 'English', 'German_Deutsch', 'Greenlandic_Inuktikut', 'Hungarian_Magyar', 'Ibibio_Efik']\
cfd = nltk.ConditionalFreqDist(\
            (lang, len(word)) [1]\
            for lang in languages\
            for word in udhr.words(lang + '-Latin1'))\
cfd.tabulate(conditions=['English', 'German_Deutsch'], samples=range(10), cumulative=True)

result ->
                  0    1    2    3    4    5    6    7    8    9\
       English    0  185  525  883  997 1166 1283 1440 1558 1638\
German_Deutsch    0  171  263  614  717  894 1013 1110 1213 1275

We can see word's length in udhr text for each Eng, Ger.\
'-Latin1' postfix added to open latin decoded file.\
Conditions variable was used to select whatever we wanna see.

### 2.4 Generating Random Text With Bigrams
bigrams() function takes a list of words and builds a list of consecutive(연속되는) word pairs
![random text](./ex2_2.png)

this is how to make random text hmm...\
I think it is just collection of words which has strong consecutive...\
Not a text in real.

## 4. Lexical Resources
### 4.1 Wordlist Corpora
With the help of stopwords -high-frequency words like `the`, `to`- \
we filter out over a quarter of the words of the text.

There are also names corpus for containing 8000 first names categorized by gender.

### 4.2 A Pronouncing Dictionary
NLTK includes CMU pronouncing dictionary for US english.\
We can make program scans the lexicon to find rhyming words \
One pronounciation is spelt in several ways. \
Phones contain digits to represent primary stress, secondary stress, no stress. \
we can define function to extract stress digits who has particular pattern.

p3 = [(pron[0]+'-'+pron[2], word) [1]\
    for (word, pron) in entries\
    if pron[0] == 'P' and len(pron) == 3] [2]

cfd = nltk.ConditionalFreqDist(p3)\

for template in sorted(cfd.conditions()):\
    if len(cfd[template]) > 10:\
        words = sorted(cfd[template])\
        wordstring = ' '.join(words)\
        print(template, wordstring[:70] + "...")

This code can find all p-words consisting of three sounds.\
And group them according to their first and last sounds.

### 4.3 Comparative Wordlists.
NLTK includes so-called Swadesh wordlists. \
lists of about 200 common words in several languages. \
We can display & convery each words to language pairs.

## 5. WordNet
### 5.1 Senses and Synonyms
a. Benz is credited with the invention of the motorcar.\
b. Benz is credited with the invention of the automobile.\

this sentences has pretty much same meanings.\
in this case, we call `motocar` and `automobile` are synonyms.\
We can find synonyms from wordnet.synset. Which also has definition and examples.\

### 5.2 The WordNet Hierarchy
WordNet synsets has abstract concepts which linked together in a hierarchy.\
More ambiguous word `car` has more specific word `motocar` and so on. \
Hyponyms: more specific word for upper concepts. \
Some words are classified more than one way so has many hyponyms.

### 5.3 More Lexical Relations
holonyms: same spell or pronounciation but different meanings\
meronyms: items to their component (포함관계) \
entails: relationship b2w verbs. ex) walking entails stepping, eating entails chewing \
antonymy: hold between lemmas. (반의어)

### 5.4 Semantic Similarity
Knowing which words are semantically related is useful for indexing a collection of texts. \
That each synset has one or more hypernym paths that link it to a root hypernym. \
So more close branch it has, more similar words.

a function `path_similarity` assigns a score in the range 0-1 based on the shortest path \
which connects the concepts in the hypernym hierarchy.\
(-1 returns where a path cannot be found.)