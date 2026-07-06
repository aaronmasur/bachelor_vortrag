# 30 minute bachelor thesis presentation - example talking points

## 0:00-1:00 - Title
Open with: "This thesis is not trying to build a new state-of-the-art RNA folding tool. I wanted to isolate architecture choice and ask what plain networks can learn by themselves."

## 1:00-3:00 - Motivation and research questions
Explain that modern RNA secondary-structure tools often report strong scores, but their performance can come from many places: preprocessing, biological constraints, postprocessing, and dataset biases. Then state the two research questions: how much data the architectures need, and how they behave under controlled structural or sequence biases.

## 3:00-8:00 - RNA background
Introduce RNA as a sequence over A, C, G, U. Explain secondary structure as base-pair formation and why structure matters for function. Use the constraints figure to show that a valid RNA secondary structure is not arbitrary: canonical pairs, minimum loop length, one partner per base, and no pseudoknots in this simplified setting.

## 8:00-10:00 - Base-pair matrix
Explain dot-bracket notation briefly, then focus on the base-pair matrix. Each cell asks: does base i pair with base j? This is the representation predicted by the models. Mention sparsity: most cells are zero.

## 10:00-14:00 - The architectures
For FFN: fully connected, fixed input length, no built-in locality or sequence-order bias.
For CNN: kernels reuse local pattern detectors; good inductive bias for image-like matrices and local motifs.
For BLSTM: reads sequence in both directions; useful for sequential context and long-range dependencies.

## 14:00-17:00 - Methodological design and pipeline
Explain the deliberate limitations: synthetic RNAfold labels, fixed length 100, no dropout/weight decay. Then walk through the pipeline: data generation -> hyperparameter optimization -> training -> evaluation. Mention MCC and RNA-specific metrics.

## 17:00-23:00 - RQ1 results
Start with the MCC table. Main story: FFN stays almost at zero, CNN performs meaningfully already with 200 sequences and improves, BLSTM does nothing at 200/1000 but starts around 5000.
Then show predicted matrices. Explain FFN learned an average picture, CNN learned sparse diagonal-like patterns, BLSTM needed enough data before it produced meaningful structures.
Then connect this to architecture: CNN local kernels help early; BLSTM needs more examples but generalizes with fewer parameters; FFN lacks the right inductive bias.

## 23:00-28:00 - RQ2 results
Explain the biased train/eval setup. Use the average-structure table first: each dataset has a different structural distribution.
For BP-span and GC: BLSTM is especially sensitive.
For helix length: CNN is especially sensitive, likely because its kernels learn helix motifs.
For multiloops: the MCC differences are moderate, but that probably also means MCC is not sensitive enough for higher-order topology.
For FFN: most behavior can be explained by average train/eval matrix similarity.

## 28:00-30:00 - Overall conclusion and outlook
Conclusion: plain architectures alone do not match engineered RNA folding tools, but they reveal clear architecture-specific strengths and failures.
Outlook: hybrid architectures, equal parameter counts, real RNA data, variable-length RNAs, more random seeds, and better topology-aware metrics.
