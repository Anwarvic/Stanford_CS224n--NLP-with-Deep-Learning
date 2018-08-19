# Assignment #3

## Requirements

To be able to solve this assignment, we need to install a few packages which can be found in the `requirements.txt` file. The dataset is downloaded already and ready to be used. 

---

## q1

According to the first question, we are there is only one script that we need to complete implementing which is `q1_window.py` at which we are going to implement two things actually:

- The function `make_windowed_data` which has a really great description inside the script that is totally description enough.
- Most of the methods of the `WindowModel` class. This class can be implemented easily. If you got stuck at any part, just check the [parser model](https://github.com/Anwarvic/Stanford_CS224n--NLP-with-Deep-Learning/blob/master/Assignment%2002/assignment2/q2_parser_model.py) we have made in Assignment #2 putting in mind that there are minor changes like using `tf.nn.sparse_softmax_cross_entropy_with_logits` instead of `tf.nn.softmax_cross_entropy_with_logits_v2`. And, of course, the shapes of the used parameters are different as well.

One of the things that helped me understand this `WindowModel` is the following graph:

![WindowModel](http://www.mediafire.com/convkey/3627/8oj7mhrkiyfjxx1zg.jpg)

As we can see, we have a two-layer neural network where the input contains the features for our window. The input shape is `D` where `D` equal the number of features for every word in our window. So, assuming that the window size is just one (one on the left and one on the right), then `D` will equal to (*3* * *number of word feature*s * *size of word embedding*). The *number of word features* can be found in the member variable `self.config.n_word_features`, the window size can be found in `self.config.window_size` and the width of word embedding can be found in `self.config.embed_size`.

The number of hidden neurons in our hidden layer `H` is equal to the hidden size determined by the member variable `self.hidden_size`. Finally, the number of neurons in the output layer `C` can be found in the member variable `self.n_classes`.



Here, there are the outputs (for reference) that I got when running `q1_window.py`  with:

- `test1`

  ```powershell
  $ python q1_window.py test1
  ```

- `test2`

  ```powershell
  $ python q1_window.py test2
  ...
  ...
  label   acc     prec    rec     f1
  PER     0.96    0.73    0.88    0.80
  ORG     0.96    0.65    0.23    0.34
  LOC     0.98    0.84    0.82    0.83
  MISC    0.98    0.81    0.42    0.55
  O       0.96    0.96    0.99    0.97
  micro   0.97    0.92    0.92    0.92
  macro   0.97    0.80    0.67    0.70
  not-O   0.97    0.76    0.68    0.72

  INFO:Entity level P/R/F1: 0.67/0.63/0.65

  INFO:Model did not crash!
  INFO:Passed!
  ```

- `train`

  ```powershell
  $ python q1_window.py train
  ...
  ...
  DEBUG:Token-level confusion matrix:
  go\gu           PER             ORG             LOC             MISC            O
  PER             2940.00         51.00           79.00           18.00           61.00
  ORG             126.00          1637.00         118.00          71.00           140.00
  LOC             36.00           168.00          1806.00         39.00           45.00
  MISC            41.00           58.00           42.00           1016.00         111.00
  O               43.00           51.00           20.00           34.00           42611.00

  DEBUG:Token-level scores:
  label   acc     prec    rec     f1
  PER     0.99    0.92    0.93    0.93
  ORG     0.98    0.83    0.78    0.81
  LOC     0.99    0.87    0.86    0.87
  MISC    0.99    0.86    0.80    0.83
  O       0.99    0.99    1.00    0.99
  micro   0.99    0.97    0.97    0.97
  macro   0.99    0.90    0.88    0.89
  not-O   0.99    0.88    0.86    0.87

  INFO:Entity level P/R/F1: 0.80/0.83/0.82
  INFO:New best score! Saving model in results/window/20180816_170849/model.weights
  ```

- `evaluate`

  ```powershell
  $ python .\q1_window.py evaluate -m 'results\window\20180817_102055\' > 'results\window\20180817_102055\results.txt'
  INFO:Initialized embeddings.
  INFO:Building model...
  INFO:took 6.81 seconds
  INFO:tensorflow:Restoring parameters from results\window\20180817_102055\model.weights
  INFO:Restoring parameters from results\window\20180817_102055\model.weights
  ```

- `shell`

  ```powershell
  $ python .\q1_window.py shell -m 'results\window\20180816_170849\'
  INFO:Initialized embeddings.
  INFO:Building model...
  INFO:took 7.62 seconds
  INFO:tensorflow:Restoring parameters from results\window\20180816_170849\model.weights
  INFO:Restoring parameters from results\window\20180816_170849\model.weights
  Welcome!
   can use this shell to explore the behavior of your model.
   Please enter sentences with spaces between tokens, e.g.,
   input> Germany's representative to the European Union's veterinary committee.

  input> x : I visited Turkey and with Zeynep last summer
  y*:
  y': O O       LOC    O   O    MISC   O    O
  input>
  ```

  ​

---

## q2

According to the second question, there are two files that need to be done:

- `q2_rnn_cell.py`
- `q2_rnn.py`
