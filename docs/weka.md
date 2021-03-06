# WEKA

java TextDirectoriesToArffFile $cat_dir > str_corn_training.arff

           java -Xmx1024m weka.filters.unsupervised.attribute.StringToWordVector
           –b -i str_corn_training.arff -o corn_training.arff -r str_corn_test.arff –s corn_test.arff
           -R 2 -W 5000 -C -T -I -N 1 -L -M 2

weka_home=/Applications/weka-3-6-12-oracle-jvm.app/Contents/Java
export CLASSPATH=$CLASSPATH:$weka_home/weka.jar:$weka_home/libsvm.jar:$JAVA_HOME/bin

-cp /Applications/weka-3-6-12-oracle-jvm.app/Contents/Java/weka.jar

java  weka.core.converters.TextDirectoryLoader -Dfile.encoding=utf-8 -dir raw > str_test_training.arff

java  -Xmx1024m weka.filters.unsupervised.attribute.StringToWordVector  -b -i str_test_training.arff -o test_training.arff -r str_test_training.arff -s test_test.arff -R 2 -W 5000 -C -T -I -N 1 -L -M 2

 java -Xmx1024m weka.filters.unsupervised.attribute.StringToWordVector -b -i str_test_training.arff -o test_training.arff -r str_test_training.arff -s test_test.arff -R 2 -W 5000 -C -T -I -N 1 -L -M 2 -tokenizer "weka.core.tokenizers.NGramTokenizer -min 2 -max 3"

java -Xmx1024m weka.filters.supervised.attribute.AttributeSelection -S "weka.attributeSelection.Ranker -N 100"  -E "weka.attributeSelection.InfoGainAttributeEval" -b -i test_training.arff -o test_chi100_training.arff -r test_training.arff -s test_chi100_test.arff -c 1

references:
https://www.youtube.com/watch?v=IY29uC4uem8
https://www.youtube.com/watch?v=zlVJ2_N_Olo
--------------

RUN 1

java weka.core.converters.TextDirectoryLoader -Dfile.encoding=utf-8 -dir ~/work/portuguese-nlp/data/raw > ~/work/portuguese-nlp/data/v1/str_all.arff

***************************************************************************

RUN 2

Pattern .*[&/-<>#-=].*  .*[&/-<>#-=].*
Pattern used .?
not used [^\w ]*

dataset saved in pruned.arff

***************************************************************************
RUN3
Pattern .*[&/-<>#-=].*  .*[&/-<>#-=].*
Pattern used .?

IDF True
TF  True
Outputwordcounts True

***************************************************************************
RUN4
Pattern .*[&/-<>#-=].*
Pattern used .?

IDF True
TF  True
Outputwordcounts True
lowercase True

weka.filters.unsupervised.attribute.StringToWordVector -R first-last -W 1000 -prune-rate -1.0 -C -T -I -N 0 -L -stemmer weka.core.stemmers.NullStemmer -M 1 -tokenizer "weka.core.tokenizers.WordTokenizer -delimiters \" \\r\\n\\t.,;:\\\'\\\"()?!\""


***************************************************************************

RUN5

IDF True
TF  True
Outputwordcounts True
lowercase True

Stopwords added
deliminiters for tokenizer  .,;:'"()?!<>/-_=

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.899     0.108      0.894     0.899     0.896      0.886    class_irr
                 0.892     0.101      0.897     0.892     0.895      0.886    class_rel
Weighted Avg.    0.895     0.105      0.896     0.895     0.895      0.886

=== Confusion Matrix ===

   a   b   <-- classified as
 471  53 |   a = class_irr
  56 463 |   b = class_rel
***************************************************************************

RUN6

last run + Pattern .*[&/-<>#-=].*

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.916     0.104      0.899     0.916     0.907      0.895    class_irr
                 0.896     0.084      0.914     0.896     0.905      0.895    class_rel
Weighted Avg.    0.906     0.094      0.906     0.906     0.906      0.895

=== Confusion Matrix ===

   a   b   <-- classified as
 480  44 |   a = class_irr
  54 465 |   b = class_rel

***************************************************************************
***************************************************************************

RUN7

Divided cleaned
java  weka.core.converters.TextDirectoryLoader -Dfile.encoding=utf-8 -dir /Users/cagil/work/portuguese-nlp/data/parsed/divided/training > /Users/cagil/work/portuguese-nlp/data/versions/v7_str_training.arff

.,;:'"()?!  "<>/-_=" not used

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.897     0.12       0.883     0.897     0.89       0.864    class_irr
                 0.88      0.103      0.894     0.88      0.887      0.864    class_rel
Weighted Avg.    0.888     0.112      0.889     0.888     0.888      0.864

=== Confusion Matrix ===

   a   b   <-- classified as
 347  40 |   a = class_irr
  46 338 |   b = class_rel

***************************************************************************
***************************************************************************

RUN8

RUN7 + FilteredClassifier

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.814     0.086      0.905     0.814     0.857      0.856    class_irr
                 0.914     0.186      0.83      0.914     0.87       0.856    class_rel
Weighted Avg.    0.864     0.136      0.868     0.864     0.863      0.856

=== Confusion Matrix ===

   a   b   <-- classified as
 105  24 |   a = class_irr
  11 117 |   b = class_rel

***************************************************************************
***************************************************************************

RUN9

java  weka.core.converters.TextDirectoryLoader -Dfile.encoding=utf-8 -dir /Users/cagil/work/portuguese-nlp/data/parsed/full-newline > /Users/cagil/work/portuguese-nlp/data/versions/v9_str_all.arff


Patterns used .? ve .*[&/-<>#-=].* (1451 --> 1431 --> 1060)
tokenizer: .,;:'"()?!
Nltk stopwords
unicode_content.replace("\n"," ")
python main/clean.py --raw_dir data/raw --parsed_dir data/parsed/full-newline --divide 2> /tmp/err.txt

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.911     0.123      0.882     0.911     0.896      0.894    class_irr
                 0.877     0.089      0.907     0.877     0.892      0.894    class_rel
Weighted Avg.    0.894     0.106      0.894     0.894     0.894      0.894

=== Confusion Matrix ===

   a   b   <-- classified as
 470  46 |   a = class_irr
  63 449 |   b = class_rel


***************************************************************************
***************************************************************************

RUN10

RUN9 + lowercase
1400 1381 1024

***************************************************************************
***************************************************************************
RUN11

1399 1380 1023

TFIDF

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.928     0.113      0.892     0.928     0.91       0.905    class_irr
                 0.887     0.072      0.925     0.887     0.905      0.905    class_rel
Weighted Avg.    0.908     0.093      0.908     0.908     0.908      0.905

=== Confusion Matrix ===

   a   b   <-- classified as
 479  37 |   a = class_irr
  58 454 |   b = class_rel

***************************************************************************
***************************************************************************
RUN11

1399 1380 1023

TFIDF + words to keep 100 (was 1000)

137 130 86

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.944     0.119      0.889     0.944     0.915      0.92     class_irr
                 0.881     0.056      0.94      0.881     0.909      0.92     class_rel
Weighted Avg.    0.912     0.088      0.914     0.912     0.912      0.92

=== Confusion Matrix ===

   a   b   <-- classified as
 487  29 |   a = class_irr
  61 451 |   b = class_rel


***************************************************************************
***************************************************************************
RUN12

TFIDF + words to keep 130
178 170 151
./ --> .*[<>\+\-\#=\$[0-9]].*

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.926     0.125      0.882     0.926     0.904      0.9      class_irr
                 0.875     0.074      0.922     0.875     0.898      0.9      class_rel
Weighted Avg.    0.901     0.099      0.902     0.901     0.901      0.9

=== Confusion Matrix ===

   a   b   <-- classified as
 478  38 |   a = class_irr
  64 448 |   b = class_rel

***************************************************************************
***************************************************************************
RUN13

python main/clean.py --raw_dir data/raw --parsed_dir data/parsed/full-text --divide 2> /tmp/err.txt

tags are removed (was content.renderContents().decode('utf-8').strip("|\n ?") --> content.text)
TFIDF + words to keep 130

.*[<>\+\-\#=\$[0-9]].*
191 180

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.911     0.111      0.892     0.911     0.901      0.89     class_irr
                 0.889     0.089      0.908     0.889     0.898      0.89     class_rel
Weighted Avg.    0.9       0.1        0.9       0.9       0.9        0.89

=== Confusion Matrix ===

   a   b   <-- classified as
 470  46 |   a = class_irr
  57 455 |   b = class_rel

***************************************************************************
***************************************************************************
RUN14

143 135
TFIDF + words to keep 100

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.932     0.105      0.899     0.932     0.915      0.907    class_irr
                 0.895     0.068      0.929     0.895     0.911      0.907    class_rel
Weighted Avg.    0.913     0.087      0.914     0.913     0.913      0.907

=== Confusion Matrix ===

   a   b   <-- classified as
 481  35 |   a = class_irr
  54 458 |   b = class_rel

***************************************************************************
***************************************************************************
RUN15

python main/clean.py --raw_dir data/raw --parsed_dir data/parsed/full-title --divide 2> /tmp/err.txt
java  weka.core.converters.TextDirectoryLoader -Dfile.encoding=utf-8 -dir /Users/cagil/work/portuguese-nlp/data/parsed/full-title > /Users/cagil/work/portuguese-nlp/data/versions/v15_str_all.arff

with titles

TFIDF + words to keep 100

.*[<>\+\-\#=\$[0-9]].*
144 --> 135

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.94      0.09       0.913     0.94      0.926      0.917    class_irr
                 0.91      0.06       0.938     0.91      0.924      0.917    class_rel
Weighted Avg.    0.925     0.075      0.925     0.925     0.925      0.917

=== Confusion Matrix ===

   a   b   <-- classified as
 485  31 |   a = class_irr
  46 466 |   b = class_rel

***************************************************************************
***************************************************************************
RUN16

python main/clean.py --raw_dir data/raw --parsed_dir data/parsed/full-main --divide 2> /tmp/err.txt

java  weka.core.converters.TextDirectoryLoader -Dfile.encoding=utf-8 -dir /Users/cagil/work/portuguese-nlp/data/parsed/full-main > /Users/cagil/work/portuguese-nlp/data/versions/v16_str_all.arff

with main

.*[<>\+\-\#=\$[0-9]].*


=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.942     0.084      0.919     0.942     0.93       0.921    class_irr
                 0.916     0.058      0.94      0.916     0.928      0.921    class_rel
Weighted Avg.    0.929     0.071      0.929     0.929     0.929      0.921

=== Confusion Matrix ===

   a   b   <-- classified as
 486  30 |   a = class_irr
  43 469 |   b = class_rel


=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.942     0.084      0.919     0.942     0.93       0.921    class_irr
                 0.916     0.058      0.94      0.916     0.928      0.921    class_rel
Weighted Avg.    0.929     0.071      0.929     0.929     0.929      0.921

=== Confusion Matrix ===

   a   b   <-- classified as
 486  30 |   a = class_irr
  43 469 |   b = class_rel


***************************************************************************
***************************************************************************
RUN17 - 21 October 2015

. ~/work/virtuals/brazil/bin/activate
cd main
python clean.py --raw_dir ../data/raw_v2 --parsed_dir ../data/parsed/full-main_v2 --divide 2> /tmp/err.txt


java  weka.core.converters.TextDirectoryLoader -Dfile.encoding=utf-8 -dir /Users/cagil/work/portuguese-nlp/data/parsed/full-main_v2 > /Users/cagil/work/portuguese-nlp/data/versions/v17_str_all.arff

weka.filters.unsupervised.attribute.StringToWordVector -R first-last -W 100 -prune-rate -1.0 -C -T -I -N 0 -L -S -stemmer weka.core.stemmers.NullStemmer -M 1 -stopwords /Users/cagil/work/portuguese-nlp/stoplist.txt -tokenizer "weka.core.tokenizers.WordTokenizer -delimiters \" \\r\\n\\t.,;:\\\'\\\"()?!\""

weka.filters.unsupervised.attribute.StringToWordVector -R first-last -W 100 -prune-rate -1.0 -C -T -I -N 0 -L -S -stemmer weka.core.stemmers.NullStemmer -M 1 -stopwords /Users/cagil/work/portuguese-nlp/stoplist.txt -tokenizer "weka.core.tokenizers.WordTokenizer -delimiters \" \\r\\n\\t.,;:\\\'\\\"()?!\""

with main

.*[<>\+\-\#=\$[0-9]].*

sonuçlar ve model dosyada kod 17
***************************************************************************
***************************************************************************
RUN18 - 21 October 2015

RAW: not cleaned

java  weka.core.converters.TextDirectoryLoader -Dfile.encoding=utf-8 -dir /Users/cagil/work/portuguese-nlp/data/v2/raw > /Users/cagil/work/portuguese-nlp/data/versions/v18_str_all.arff

=== Confusion Matrix ===

   a   b   <-- classified as
 480  44 |   a = class_irr
  60 446 |   b = class_rel
0.902
0.896

cleaned
java  weka.core.converters.TextDirectoryLoader -Dfile.encoding=utf-8 -dir /Users/cagil/work/portuguese-nlp/data/parsed/v2 > /Users/cagil/work/portuguese-nlp/data/versions/v18_parsed_str_all.arff

F-Measure
0.929 class_irr
0.925 class_rel



***************************************************************************
***************************************************************************
RUN19 - 22 October 2015

Scheme:weka.classifiers.trees.RandomForest -I 80 -K 0 -S 1 -D
weka.classifiers.trees.RandomForest -I 70 -K 0 -S 1 -D


=== Evaluation on test split ===
=== Summary ===

Correctly Classified Instances         294               96.3934 %
Incorrectly Classified Instances        11                3.6066 %
Kappa statistic                          0.9276
Mean absolute error                      0.1987
Root mean squared error                  0.2416
Relative absolute error                 39.7267 %
Root relative squared error             48.2933 %
Total Number of Instances              305     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.963     0.035      0.969     0.963     0.966      0.991    class_irr
                 0.965     0.037      0.958     0.965     0.961      0.991    class_rel
Weighted Avg.    0.964     0.036      0.964     0.964     0.964      0.991

=== Confusion Matrix ===

   a   b   <-- classified as
 157   6 |   a = class_irr
   5 137 |   b = class_rel


How to move the class att to the end: https://youtu.be/jSZ9jQy1sfE?t=9m10s
