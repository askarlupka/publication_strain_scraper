---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.8
  kernelspec:
    display_name: pytorch_39
    language: python
    name: pytorch_39
---

### Extract PMID List from Spreadsheet

Notes:

* Emily will provide a spreadsheet of PMIDS

Possible Approach:

* Parse CSV with pandas
* Rejoice in the simplicity of pandas


### Extract Publication Data from Pubmed and/or PMC

* https://pubmed.ncbi.nlm.nih.gov/
* https://www.ncbi.nlm.nih.gov/pmc/

Notes:

* Citations are not necessarily present in both systems.
* PMIDs can exist in both systems so we may need to figure out where there is overlap.

Possible Approach:

* Build a pandas dataframe.  Each row contains a PMID, a link to the raw data if possible, and flags to indicate whether or not the publication is behind a paywall or only available in PDF format.
    * We can develop logic to decide if a publication has scrapable text.  Maybe by looking at the full text links and seeing if they contain a PMC link or by looking at the extension of linked text to see if it's PDF.  There are many possibilites.  
* Starting with non-PDFs, develop an approach to extract the text from Pubmed.  We can probably avoid using Selenium as that's really more if you have to log in to a system.  With Pubmed, once we have a URL to the data we can use Beautiful Soup to extract.  
    * Take https://pubmed.ncbi.nlm.nih.gov/34603333/ as an example, the text is available via a link on that page to https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8481571/.  
    


### Extract Genbank, SRA, GEO, and PDB accession numbers

Notes:

* Discover the format of the various accession numbers.  How can we best identify them in a publication?  Do we search for words like PDB or is the format of the accession enough to differentiate it from random text?

Possible Approach:

* Develop a set of rules to decide how to extract accession identifiers
* Apply those rules to the Pubmed data scraped above.  Tools like `spacy` can help tokenize large bodies of text and even do things like string matching.  
* Store all accession numbers in some type of data structure.  Because we're potentially collecting many values, it might be helpful to store data in a dictionary or JSON structure.  We (DIFZ) use unstructured data stores for a lot of our work.



### Extract Strains

Extend **Extract Genbank, SRA, GEO, and PDB accession numbers** to work on Strain Names


### Extract Other?

Are there other types of data you think might be worth extracting from the text?


## Outputs

* % of publications with open-access text
* How to handle PDFs vs Websites?  Could be a simple write up, could be code
* Extraction of Accessions
* Extraction fo Strains (time permitting)


## Results and Conclusions

This is where we need your input.  What makes the most sense for your time at DIFZ?  For example writing out the overall approach, steps, output, next steps, and challenges (e.g. open vs. close access, pdf vs web scraping).  

```python

```
