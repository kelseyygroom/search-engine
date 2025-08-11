# Search Engine 

## Overview

This project implements a search engine from scratch, designed to index and retrieve web pages from a large corpus nuder realistic performance and memory constraints.

This system is divided into two main components:
1. Indexer: parses and processes a corpus of HTML pages, create an efficient disk-backed inverted index.
2. Search Component: processes user queries, ranks relevant results using TF-IDF and word importance scoring, and returns URLs sorted by relevance.

This project supports large-scale datasets (upwards of 50,000 documents) and ensures query responses in under 300ms by offloading index data to disk and reading only necessary portions during retrieval.

## Indexer

For the indexer portion of this program, I parsed over 50,000 HTML documents, tokenizing them and periodically storing them in 27 mini inverted index (one for each letter of the alphabet + numbers/special characters) on the disk. This partial index offloading was used to simulate memory-limited environments. 

Throughout the indexing process, I maintained a document id index, where each url was associated with a document id. This reduced the size of the main index, because each term is associated with document ids rather than urls.

After all urls were parsed, I merged them all into the singular main index. During this process, I also constructed a positions index where each term was associated with a byte offset (corresponding to its position in the main inverted index). This allowed me to seek specific terms within the inverted index, reducing the time spent searching through the inverted index file, and the need for loading too much of the inverted index into main memory. 

I also calculate tf-idf scores for each term in a document.


## Search Engine

The search component reads a query from the console, stems and tokenizes terms, and then retrieves all relevant document ids from the inverted index. It narrows down the document ids to show only those that are common among all query terms. Based on tf-idf scores, results are returned for the query sorted by relevance.
