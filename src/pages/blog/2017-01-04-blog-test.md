---
templateKey: blog-post
title: New Blog Being Tested!
date: 2017-04-26T15:00:10+01:00
description: A brief test of using the blog...
tags:
  - nlp
  - word embeddings
  - R
---
Word embeddings can be thought of as a dimension-reduction tool, needing a sequence of tokens to learn from. They are really that generic, but I’ve only ever heard of them used for languages; i.e. the sequences are sentences, the tokens are words (or compound words, or n-grams, or morphemes).

This blog post is for code I presented recently on how to use the H2O implementation of word embeddings, aka word2vec. The main thing being demonstrated is that they apply equally well for any language, but you may need some language-specific tokenization, and other data engineering, first.

Here is the preparation code, in R; bring in H2O, and define a couple of helper functions.

```R
library(h2o)

h2o.init(nthreads = -1)

show <- function(w, v){
  x = as.data.frame(h2o.cbind(w, v))
  x = unique(x)
  plot(x[,2:3], pch=16, type="n")
  text(x[,2:3], x[,1], cex = 2.2)
}

reduceShow <- function(w,v){
m <- h2o.prcomp(v, 1:ncol(v), k = 2, impute_missing=T)
p <- h2o.predict(m, v)
show(w, p)
}
```

