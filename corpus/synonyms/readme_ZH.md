# 同义词库文件夹说明

该文件夹用于存储不同语言的同义词库文件，每个文件以 ver_01.jsonl 类似的格式命名，其中每一行存储一对同义词。以下是文件夹结构：

└─synonyms
    ├─EN
    └─ZH

EN 文件夹存储英文同义词库文件。
ZH 文件夹存储中文同义词库文件。
读取同义词库文件的代码示例：


```python



import jsonlines

def read_synonyms(file_path):
    synonyms = []
    with jsonlines.open(file_path) as reader:
        for line in reader:
            synonyms.append(line)
    return synonyms

# 读取英文同义词库文件
en_synonyms = read_synonyms("synonyms/EN/ver_01.jsonl")

# 读取中文同义词库文件
zh_synonyms = read_synonyms("synonyms/ZH/ver_01.jsonl")

```
