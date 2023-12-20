# Synonyms Folder Description

This folder is used to store synonym files in different languages. Each file is named in the format of ver_01.jsonl and stores one synonym pair per line. The folder structure is as follows:

└─synonyms
    ├─EN
    └─ZH

The EN folder stores synonym files in English.
The ZH folder stores synonym files in Chinese.
Example code for reading synonym files:

```python

import jsonlines

def read_synonyms(file_path):
    synonyms = []
    with jsonlines.open(file_path) as reader:
        for line in reader:
            synonyms.append(line)
    return synonyms

# Read English synonym file
en_synonyms = read_synonyms("synonyms/EN/ver_01.jsonl")

# Read Chinese synonym file
zh_synonyms = read_synonyms("synonyms/ZH/ver_01.jsonl")

```