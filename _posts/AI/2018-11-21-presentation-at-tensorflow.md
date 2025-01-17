---
date: 2018-11-21
title:  "Learning Language with BERT"
tagline: Presentation
category: AI
tags:
- Presentation
- TensorFlow
- Magenta
layout: post
published: true
---
{% include JB/setup %}



### Presentation Link

Sam Witteveen and I started the TensorFlow and Deep Learning Singapore group on MeetUp in February 2017,
and [the twentieth MeetUp, aka TensorFlow and Deep Learning: Happy Birthday TensorFlow](https://www.meetup.com/TensorFlow-and-Deep-Learning-Singapore/events/256431012/),
was again hosted by Google Singapore.

We were honoured to have two Google Brain team members speak at the event : 

*  Rachel Lim talking about tf.data ("Beyond the Basics"); and
*  Chuan Yu Foo talking about TFX ("Complete ML Pipelines")

For my part, I gave a talk titled "Language Learning with BERT", which discussed the basics of Google's newly released BERT 
natural language processing model.

Sam rounded out the evening with an entertaining look at the Big-GAN models available on TF-Hub.

<!--

"Learning Language with BERT" - Martin Andrews

In this talk for people just starting out, 
Martin will describe how Google's new BERT model 
can turbo charge your Natural Language Processing solutions.


Outline:

  Old Style
    Embeddings
    BiDirectional LSTM layers 
      https://www.researchgate.net/figure/Word-embeddings-are-fed-to-a-bidirectional-LSTM-where-V-f-V-b-respectively-represent_fig4_316863199
    Train from scratch
  
  New Style
    SentencePiece
    Language Model objective
    FineTuning
  
  Newest version
    http://muppet.wikia.com/wiki/Bert
    https://www.reddit.com/r/MachineLearning/comments/9t01to/p_official_bert_tensorflow_code_pretrained_models/
    'Unsupervised' training 
    
  Code etc

  ImageNet moment




  Paper: 
    https://arxiv.org/abs/1810.04805
    
    AIAYN : https://arxiv.org/abs/1706.03762
    
    
  Code: 
    https://github.com/google-research/bert#what-is-bert
    
  TensorFlow:
    https://github.com/google-research/bert
    
  PyTorch (install via pip):
    https://github.com/huggingface/pytorch-pretrained-BERT



  Next steps : 
    We are planning on releasing a multilingual model soon (big shared WordPiece vocab trained on 60 languages, with special handling of Chinese).



Advertise 
  Deep Learning Developer Module 1 : JumpStart
  Deep Learning Developer Module 2+ 
  TF&DL next == ?
  Interns
  
!-->


The slides for my talk are here :

<a href="http://redcatlabs.com/2018-11-21_TFandDL_BERT/" target="_blank">
![Presentation Screenshot]({{ site.url }}/assets/img/2018-11-21_TFandDL_BERT_600x390.png)
</a>

If there are any questions about the presentation please ask below, 
or contact me using the details given on the slides themselves.

<a href="http://redcatlabs.com/2018-11-21_TFandDL_BERT/#/5" target="_blank">
![Presentation Content Example]({{ site.url }}/assets/img/2018-11-21_TFandDL_BERT_5_600x390.png)
</a>

### Video Link

The presentation was kindly <a href="https://www.engineers.sg/video/language-learning-with-bert-tensorflow-and-deep-learning-singapore--2982" target="_blank">recorded by Engineers.sg</a>.


PS:  And if you liked the content, please 'star' my <a href="https://github.com/mdda/deep-learning-workshop" target="_blank">Deep Learning Workshop</a> repo ::
<!-- From :: https://buttons.github.io/ -->
<!-- Place this tag where you want the button to render. -->
<span style="position:relative;top:5px;">
<a aria-label="Star mdda/deep-learning-workshop on GitHub" data-count-aria-label="# stargazers on GitHub" data-count-api="/repos/mdda/deep-learning-workshop#stargazers_count" data-count-href="/mdda/deep-learning-workshop/stargazers" data-icon="octicon-star" href="https://github.com/mdda/deep-learning-workshop" class="github-button">Star</a>
<!-- Place this tag right after the last button or just before your close body tag. -->
<script async defer id="github-bjs" src="https://buttons.github.io/buttons.js"></script>
</span>

