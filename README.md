# Sentiment data cleaning scripts
The scripts are seperated into 3 types, each for different data sources (Facebook, News, blogs, Youtube and novels) - 
1. 0-* (dump-csv)
2. 1-* (clean-text)
3. 2-* (lemmatize-and-deduplicate)

They are intended to be run in one after another.

The `dump-csv` (0-*) scripts produce a common format CSV file in the `output` directory (file name with pattern 0-\*) by reading raw files in different formats from the `input` directory. 

Then it's consumed by `clean-text` (1-\*) scripts. The clean-text scripts produce 3 CSV files, each for documents, paragraphs and sentences in the `output` directory (file names with pattern 1-\*). 

The files produced by `clean-text` scripts are consumed by `lemmatize-and-deduplication` script. This script also outputs 3 files for documents, paragraphs and sentences (file names with 2-\* pattern).

A brief description of what 3 types of scripts do follows -
## 0-* Scripts: dump-csv
This notebook takes the raw files (JSON in case of blog, multiple CSV files in case of youtube crawl data) from different sources, and extract only the relevant information from them (Text, Source, URL etc) and combine them into a single CSV file (in a common format) to be processed by Script 1.

## 1-* Scripts: clean-texts-source
This notebook takes the csv files from different sources containing raw and unfiltered text (Output of script 0), and perform 2 major operations to make the quality of the unlabeled data better.
1. Cleaning & Filtering: This notebook first cleans the text by removing repeating punctuation and emoticons. It also filters out all the non-bangla sentences in the text as we will build a purely bangla dataset for sentiment analysis. 
2. Extracting Documents, Paragraphs and Sentences: Then it extracts documents of moderate length by splitting the document into paragraphs and allowing documents with no more than 10 paragraphs. Then it extracts the sentences as well.


## 2-* Scripts: lemmatize-and-deduplicate-data
This notebook takes all the csv files containing clean and filtered text (Output of script 2), and perform 3 more operations to make the quality of the unlabeled data even better.
1. Random Shuffling: To make the distribution of the training data more diverse (Social media, Newspaper etc.)
2. Lemmatization: Lemmatization helps us to achieve the root forms of the inflected (derived) words. We have used Banglakit lemmatizer for this project.
3. Filtering based on Jaccard Similarity : To avoid putting many similar sentences in the dataset we have then filtered out all the sentence that has a similarity score of 70% or more after performing lemmatization.


