# Anserini Regressions: BEIR (v1.0.0) &mdash; CQADupStack-mathematica

This page documents BM25 regression experiments for [BEIR (v1.0.0) &mdash; CQADupStack-mathematica](http://beir.ai/).
These experiments index the corpus in a "flat" manner, by concatenating the "title" and "text" into the "contents" field.

The exact configurations for these regressions are stored in [this YAML file](../../src/main/resources/regression/beir-v1.0.0-cqadupstack-mathematica-flat.yaml).
Note that this page is automatically generated from [this template](../../src/main/resources/docgen/templates/beir-v1.0.0-cqadupstack-mathematica-flat.template) as part of Anserini's regression pipeline, so do not modify this page directly; modify the template instead.

From one of our Waterloo servers (e.g., `orca`), the following command will perform the complete regression, end to end:

```
python src/main/python/run_regression.py --index --verify --search --regression beir-v1.0.0-cqadupstack-mathematica-flat
```

## Indexing

Typical indexing command:

```
target/appassembler/bin/IndexCollection \
  -collection BeirFlatCollection \
  -input /path/to/beir-v1.0.0-cqadupstack-mathematica-flat \
  -index indexes/lucene-index.beir-v1.0.0-cqadupstack-mathematica-flat/ \
  -generator DefaultLuceneDocumentGenerator \
  -threads 1 -storePositions -storeDocvectors -storeRaw \
  >& logs/log.beir-v1.0.0-cqadupstack-mathematica-flat &
```

For additional details, see explanation of [common indexing options](../../docs/common-indexing-options.md).

## Retrieval

Topics and qrels are stored [here](https://github.com/castorini/anserini-tools/tree/master/topics-and-qrels), which is linked to the Anserini repo as a submodule.

After indexing has completed, you should be able to perform retrieval as follows:

```
target/appassembler/bin/SearchCollection \
  -index indexes/lucene-index.beir-v1.0.0-cqadupstack-mathematica-flat/ \
  -topics tools/topics-and-qrels/topics.beir-v1.0.0-cqadupstack-mathematica.test.tsv.gz \
  -topicreader TsvString \
  -output runs/run.beir-v1.0.0-cqadupstack-mathematica-flat.bm25.topics.beir-v1.0.0-cqadupstack-mathematica.test.txt \
  -bm25 -removeQuery -hits 1000 &
```

Evaluation can be performed using `trec_eval`:

```
tools/eval/trec_eval.9.0.4/trec_eval -c -m ndcg_cut.10 tools/topics-and-qrels/qrels.beir-v1.0.0-cqadupstack-mathematica.test.txt runs/run.beir-v1.0.0-cqadupstack-mathematica-flat.bm25.topics.beir-v1.0.0-cqadupstack-mathematica.test.txt
tools/eval/trec_eval.9.0.4/trec_eval -c -m recall.100 tools/topics-and-qrels/qrels.beir-v1.0.0-cqadupstack-mathematica.test.txt runs/run.beir-v1.0.0-cqadupstack-mathematica-flat.bm25.topics.beir-v1.0.0-cqadupstack-mathematica.test.txt
tools/eval/trec_eval.9.0.4/trec_eval -c -m recall.1000 tools/topics-and-qrels/qrels.beir-v1.0.0-cqadupstack-mathematica.test.txt runs/run.beir-v1.0.0-cqadupstack-mathematica-flat.bm25.topics.beir-v1.0.0-cqadupstack-mathematica.test.txt
```

## Effectiveness

With the above commands, you should be able to reproduce the following results:

| **nDCG@10**                                                                                                  | **BM25**  |
|:-------------------------------------------------------------------------------------------------------------|-----------|
| BEIR (v1.0.0): CQADupStack-mathematica                                                                       | 0.2015    |
| **R@100**                                                                                                    | **BM25**  |
| BEIR (v1.0.0): CQADupStack-mathematica                                                                       | 0.4877    |
| **R@1000**                                                                                                   | **BM25**  |
| BEIR (v1.0.0): CQADupStack-mathematica                                                                       | 0.7221    |
