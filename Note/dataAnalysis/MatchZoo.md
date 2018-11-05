#MatchZoo

> https://github.com/faneshion/MatchZoo/

### MatchZoo 简介

matchzoo 是一个深度文本匹配的一个工具箱，它实现了现在最新的10个深度文本匹配模型，它能应用在转述，文本蕴含，问答，对话，信息检索上。

###Input File

- **corpus.txt**: Each line is corresponding to a document. The first column is document ID. Then the following words are from this document after tokenization.

​     TID (文本id) txt_token （文本tokened)  

> 这个文件应该是所有的 问题，参考答案，真实回答

- **corpus_preprocessed.txt**: Each line is corresponding to a document. The first column is document id. The second column is the document length, followed by the ids of words in this document.

  TID （文本id） 字典id序列


- **relation_train.txt/relation_valid.txt/relation_test.txt**: Each line is "label query_id doc_id", which could be used for experiments including document retrieval, passage retrieval, answer sentence selection, etc. For each query, the documents are sorted by the labels. These labels can be binary or multi-graded. 

- **sample.txt**: Each line is the raw query and raw document text of a document. The format is "label \t query \t document_txt".

- **word_dict.txt**: The word dictionary. Each line is the word and word_id.

  词典 : word word_id 

- **corpus_preprocessed_dssm.txt/word_dict_dssm.txt**: These files have the same format with "corpus_preprocessed.txt/word_dict.txt". Since DSSM uses tri-letter based dictionary, we created seperated files for the word dictionary for DSSM model.

功能

| Tasks                      | Text 1   | Text 2     | Objective              |
| -------------------------- | -------- | ---------- | ---------------------- |
| Paraphrase Indentification | string 1 | string 2   | classification         |
| Textual Entailment         | text     | hypothesis | classification         |
| Question Answer            | question | answer     | classification/ranking |
| Conversation               | dialog   | response   | classification/ranking |
| Information Retrieval      | query    | document   | ranking                |

使用

