 do_preprocess: True或False，用于对ConvMF的原始数据进行预处理。

raw_rating_data_path:原始评级数据路径的路径。数据格式应该是用户id::item id::rating。

min_rating:小于min_rating级别的用户将被删除。

max_length_document:每个项目的文档的最大长度。

max_df:忽略文档频率高于给定值的术语的阈值。例如，删除语料库停止词。

vocab_size:词汇量的大小。

split_ratio:将整个数据集的1-ratio、ratio/2和ratio/2分别构造为训练集、有效集和测试集。

data_path:到培训、有效和测试数据集的路径。

欧塞尔路径:路径到R, D_all在预处理步骤中生成的集合。

res_dir:指向ConvMF结果的路径

emb_dim:字向量的潜在维度大小。

pretrain_w2v:用于初始化字向量的预训练字嵌入模型的路径。word to vector

give_item_weight:为R-ConvMF赋值时为真或假。

维度:用户和项目的潜在维度的大小。

lambda_u:用户正则化器参数。

lambda_v:项目正则化器的参数。

max_iter:迭代的最大次数。

num_kernel_per_ws: CNN模块的每个窗口大小的内核数量。



usage: run.py [-h] [-c DO_PREPROCESS] [-r RAW_RATING_DATA_PATH] [-i RAW_ITEM_DOCUMENT_DATA_PATH] [-m MIN_RATING]
              [-l MAX_LENGTH_DOCUMENT] [-f MAX_DF] [-s VOCAB_SIZE] [-t SPLIT_RATIO] [-d DATA_PATH] [-a AUX_PATH]
              [-o RES_DIR] [-e EMB_DIM] [-p PRETRAIN_W2V] [-g GIVE_ITEM_WEIGHT] [-k DIMENSION] [-u LAMBDA_U]
              [-v LAMBDA_V] [-n MAX_ITER] [-w NUM_KERNEL_PER_WS]



参数|默认|

| --------------------------------------------------- | ------- |

| ' -h '， '——帮助' | {}|

| ' -c '， '——do_preprocess ' | ' False ' |

| - r <路径>,”——raw_rating_data_path <路径> ' | {}|

| - i <路径>,”——raw_item_document_data_path <路径> ' | {}|

| ' -m '， '——min_rating ' | {} |

-l ——max_length_document <整数> ' | 300 |

| ' -f '， '——max_df ' | 0.5 |

| ' -s '， '——vocab_size ' | 8000 |

| ' -t '， '——split_ratio ' | 0.2 |

| ' -d '， '——data_path ' | {} |

| ' -a '， '——aux_path ' | {} |

| ' -o '， '——res_dir ' | {} |

| ' -e <整数> '，'——emb_dim <整数> ' | 200 |

| ' -p '， '——pretrain_w2v ' | {} |

| ' -g '， '——give_item_weight ' | ' True ' |

| ' -k <整数> '，'——维数<整数> ' | 50 |

| ' -u '， '——lambda_u ' | {} |

| ' -v '， '——lambda_v ' | {} |

| ' -n <整数> '，'——max_iter <整数> ' | 200 |

| ' -w <整数> '，'——num_kernel_per_ws ' | 100 |


 python3 run.py -d test/movielens-10m -a test/ml-10m/ -c True -r test/movielens-10m/ratings.dat -i test/movielens-10m/movies.dat -m 1

python3 run.py -d test/movielens-1m -a test/ml-1m -u 100 -v 10 -o result-ml-1m



 python3 run.py -d test/aiv/preprocessed/cf/0.2_1 -a test/aiv/preprocessed -u 1 -v 100 -o result-aiv2 -p test/glove.6B.200d.txt