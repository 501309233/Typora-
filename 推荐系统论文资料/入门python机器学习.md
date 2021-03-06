[toc]

# TF-IDF

TF-IDF(term frequency-inverse document frequency)词频-逆向文件频率。在处理文本时，如何将文字转化为模型可以处理的向量呢？TF-IDF就是这个问题的解决方案之一。字词的重要性与其在文本中出现的频率成正比(TF)，与其在语料库中出现的频率成反比(IDF)。

* TF

词频。TF(w) = (词w在文档中出现的次数)/(文档的总词数)

* IDF

逆向文件频率。有些词可能在文本中频繁出现，但并不重要，也即信息量小，如is,of,that这些单词，这些单词在语料库中出现的频率也非常大，我们就可以利用这点，降低其权重。IDF(w)=log_e(语料库的总文档数)/(语料库中词w出现的文档数)

* TF-IDF

将TF和IDF相乘就得到了综合参数：TF-IDF = TF*IDF