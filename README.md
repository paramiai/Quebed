# Quebed

**Quebed** is a QA system that aims at finding the most relavent answer(s) from a pool of embedded answers/paragraphs, when given an embeded question.

With reference to [DocProduct's](https://github.com/re-search/DocProduct), we train our model by inputting pairs of QA from SQuAD dataset into separate roBERTa pre-trained models for embedding. The input of answer model are paragraphs that contains answer inforamtion with the answer span bracketed in ```<s> and </s>``` tokens, i.e. ```<s> Paragrah... <s> asnwer span </s> ... Paragraph </s>```.

To test the model, answers/paragraphs are first embedded in a map. With the help of faiss, the most revalent answers can then be found when an embeded question is given. 

</br>

### Training experiment result

**Best version:** Pooling question embeddings and pooling paragraph embeddings with answer span bracketed in ```<s>``` and ```</s>``` tokens (v4.2)

|  bs |  lr  | epoch | Accuracy: top 3 |  top 5 | top 10 | top 20 | top 50 |
|:---:|:----:|:-----:|:---------------:|:------:|:------:|-------:|:------:|
|  48 | 6e-5 |   6   |     74.26%      | 80.38% | 86.98% | 92.12% | 95.95% |

</br>

- v1: Averaging question embeddings and averaging answer span embeddings

|  bs |  lr  | epoch | Accuracy: top 3 | top 5 | top 10 | top 20 | top 50 |
|:---:|:----:|:-----:|:---------------:|:-----:|:------:|-------:|:------:|
| 128 | 3e-5 |   1   |      1.00%      | 2.59% |  7.51% | 20.36% | 49.69% |

</br>

- v2.1: Pooling question embeddings and averaging answer span embeddings

|  bs |   lr   | epoch | Accuracy: top 3 |  top 5 | top 10 | top 20 | top 50 |
|:---:|:------:|:-----:|:---------------:|:------:|:------:|:------:|:------:|
| 128 |  3e-5  |   1   |      6.30%      | 10.45% | 20.09% | 36.02% | 95.10% |
|  32 |  3e-5  |   1   |      11.87%     | 17.88% | 30.85% | 48.92% | 82.92% |
|  32 | 4.5e-5 |   1   |      16.38%     | 23.28% | 35.53% | 48.85% | 67.27% |
|  32 |  6e-5  |   1   |      10.45%     | 17.79% | 30.68% | 46.04% | 68.71% |
|  48 | 1.5e-5 |   1   |      3.27%      |  4.76% |  8.97% | 15.90% | 32.10% |
|  48 |  3e-5  |   1   |      9.59%      | 14.27% | 23.47% | 35.56% | 54.53% |
|  48 | 4.5e-5 |   1   |      11.93%     | 19.55% | 31.58% | 44.99% | 65.10% |
|  48 |  6e-5  |   1   |      20.75%     | 29.22% | 41.64% | 55.78% | 73.42% |
|  48 | 6.5e-5 |   1   |      16.85%     | 23.91% | 37.19% | 52.38% | 72.55% |
|  16 |  3e-5  |   1   |      17.33%     | 23.24% | 34.71% | 49.25% | 67.52% |
|  16 |  4e-5  |   1   |      20.02%     | 27.64% | 39.40% | 52.90% | 72.65% |
|  16 | 4.5e-5 |   1   |      20.19%     | 27.39% | 39.55% | 52.96% | 70.86% |
|  64 |  6e-5  |   1   |      14.56%     | 21.49% | 32.56% | 46.48% | 65.19% |
|  64 |  7e-5  |   1   |      6.84%      | 10.49% | 17.98% | 28.06% | 47.46% |
|  48 |  6e-5  |   2   |      27.87%     | 36.55% | 49.21% | 63.08% | 78.95% |
|  48 |  6e-5  |   3   |      40.41%     | 50.36% | 63.10% | 74.95% | 87.09% |
|  48 |  6e-5  |   4   |      45.97%     | 55.11% | 67.69% | 78.54% | 88.99% |

</br>

- v2.2: Pooling question embeddings and averaging answer span embeddings (locate answer with tokenizer's offset_mapping)

|  bs |   lr   | epoch | Accuracy: top 3 |  top 5 | top 10 | top 20 | top 50 |
|:---:|:------:|:-----:|:---------------:|:------:|:------:|:------:|:------:|
|  48 |  6e-5  |   1   |      22.34%     | 31.26% | 45.29% | 59.86% | 77.18% |
|  48 |  6e-5  |   3   |      45.75%     | 54.71% | 68.10% | 78.68% | 89.79% |
|  48 |  6e-5  |   4   |      47.45%     | 56.22% | 68.49% | 79.35% | 90.47% |
|  48 |  6e-5  |   5   |      53.74%     | 63.06% | 74.17% | 83.05% | 91.23% |
|  48 |  6e-5  |   6   |      52.73%     | 61.39% | 72.81% | 82.39% | 90.82% |

</br>

- v3: v2 + using ```deepset/roberta-base-squad2``` model

|  bs |   lr   | epoch | Accuracy: top 3 |  top 5 | top 10 | top 20 | top 50 |
|:---:|:------:|:-----:|:---------------:|:------:|:------:|:------:|:------:|
|  32 |  3e-5  |   1   |      19.94%     | 27.33% | 39.59% | 53.31% | 70.80% |
|  32 | 4.5e-5 |   1   |      22.08%     | 30.95% | 44.99% | 58.89% | 75.83% |
|  32 |  6e-5  |   1   |      28.64%     | 37.96% | 52.72% | 66.89% | 81.87% |
|  32 |  7e-5  |   1   |      22.89%     | 30.62% | 42.53% | 56.22% | 72.71% |
|  48 | 4.5e-5 |   1   |      25.22%     | 33.42% | 46.34% | 60.85% | 77.58% |
|  48 |  6e-5  |   1   |      25.30%     | 33.27% | 46.07% | 60.51% | 76.64% |
|  48 |  7e-5  |   1   |      28.80%     | 37.38% | 50.27% | 63.12% | 79.55% |
|  48 |  8e-5  |   1   |      25.05%     | 33.60% | 46.83% | 59.82% | 75.37% |
|  32 |  6e-5  |   3   |      41.48%     | 50.29% | 62.08% | 72.45% | 84.68% |
|  32 |  6e-5  |   4   |      45.18%     | 54.05% | 65.65% | 76.33% | 87.25% |
|  32 |  6e-5  |   5   |      37.04%     | 44.32% | 55.41% | 67.43% | 81.93% |
|  48 |  7e-5  |   3   |      43.35%     | 52.13% | 63.11% | 75.19% | 86.47% |
|  48 |  7e-5  |   4   |      48.28%     | 56.73% | 68.15% | 78.00% | 88.36% |
|  48 |  7e-5  |   5   |      42.54%     | 50.29% | 60.54% | 70.66% | 83.40% |

</br>

- v4.1: Pooling question embeddings and pooling paragraph embeddings with answer span bracketed in ```**``` and ```##``` tokens.

|  bs |  lr  | epoch | Accuracy: top 3 |  top 5 | top 10 | top 20 | top 50 |
|:---:|:----:|:-----:|:---------------:|:------:|:------:|-------:|:------:|
|  48 | 6e-5 |   1   |     55.28%      | 63.95% | 74.49% | 82.71% | 91.11% |

</br>

- v4.2: Pooling question embeddings and pooling paragraph embeddings with answer span bracketed in ```<s>``` and ```</s>``` tokens.

|  bs |  lr  | epoch | Accuracy: top 3 |  top 5 | top 10 | top 20 | top 50 |
|:---:|:----:|:-----:|:---------------:|:------:|:------:|-------:|:------:|
|  48 | 6e-5 |   1   |     57.39%      | 65.50% | 75.67% | 83.59% | 91.53% |
|  48 | 6e-5 |   3   |     69.28%      | 76.82% | 84.33% | 89.79% | 95.55% |
|  48 | 6e-5 |   4   |     71.41%      | 78.41% | 85.07% | 91.10% | 96.07% |
|  48 | 6e-5 |   5   |     72.44%      | 79.08% | 85.44% | 90.81% | 95.16% |
|  48 | 6e-5 |   6   |     74.26%      | 80.38% | 86.98% | 92.12% | 95.95% |
|  48 | 6e-5 |   7   |     73.89%      | 80.45% | 87.38% | 91.94% | 96.05% |

