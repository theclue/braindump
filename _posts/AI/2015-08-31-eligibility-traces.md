---
layout: post
category: AI
title: Eligibility Traces and Neural Networks
tagline: Performance-based rewards
date: 2015-08-31
tags: [NeuralNetworks,ReinforcementLearning]
published: false
---
{% include JB/setup %}

Musings : Eligibility Traces
------------------------------------------------

Several things that bubbled to the surface during the Reinforcement Learning course:

*  Eligibility traces, which are a way of keeping track of how rewards for TD(\lambda) should be attributed
to the states/actions that lead to them seem very reminiscent of activation potentials

*  Also, aiming for minimum fit error is the same as optimising for least surprise, which is the same as
making predictions that are as un-surprising as possible

*  Distributing the amount of 'surprise' (positive or negative) to exponentially decaying eligibility
nodes with a network seems like a very natural reinterpretation of the TD(\lambda) framework

*  But current neural networks are densified, with multi-dimensional spaces embedding knowledge
in a way that is entangled far more thoroughly than the sparse representations found in the brain
would suggest

*  Perhaps the 'attribution problem' that sparse+eligibility solves is equivalent to dense+backprop  

*  But there is a definite disconnect between the rationale between the two methods that requires
more justification



Musings : Gimbal Learning
------------------------------------------------

Looks like [Lecture 8 of the Reingforcement Learning  Course by David Silver](rl-course-by-david-silver-lecture-8-integrating-learning-and-planning-2/)
is super-relevant to the Gimbal control problem that I've been considering.  

It's also interesting that the things I had already assumed would have been obvious 
are currently considered state-of-the-art.

In summary : 

*  Learn model from real world by observing state transitions, and then learning {state, action}-to-state mapping
   +  Also learn {State, Action}-to-Reward mapping (almost a separate model)
*  Apply model-free methods to model-simulated world
*  Once 'correct' action has been selected, actually perform it
*  Now we have new real-world learning with which to fine-tune the world model

To apply this to gimbal, seems like one could present 'target trajectories' to 
controller in turn, letting it learn a common world-model, 
with different reward-models for each goal.  And let it self-play...



Musings : Exploration vs Exploitation
------------------------------------------------

*  Interesting that $(\mu + n\sigma)$ is an effective action-chooser, since that is what I was doing in the 1990s,
   but using as a placeholder heuristic until I found something that was 'more correct'
   
*  But $\[0,1\]$ bounded i.i.d variables having a decent confidence bound (Hoeffding's inequality) was a new thing for me

*  Also, liked the Thompson sampling (from 1930s) that implicitly created samples according to a distribution, 
   merely by using samples from the underlying factors is very elegant
   + and makes me think about the heuristic in Genetic Algorithms for 'just sampling' 
   as potentially solving a (very complex) probablity problem, without actually doing any computation
   


Musings : Games and State-of-the-Art
------------------------------------------------

*  The tree optimisations seem 'obvious' in retrospect - but clearly each was a major 'aha!' when it 
   was first proposed.  Very interesting.

*  Almost all of the State-of-the-Art methods use binary features, linear approximations to v*(), search and self-play.

*  Perhaps deeper models will work instead of the linear ones - but it's interesting that binary (~sparse?)
   features are basically powerful enough (when there's some tree-search for trickier strategy planning)



Musings :ICLR
------------------------


*  Refactoring a complex model into a simpler/sparser one seems likely to have generalization benefits,
since Occam's razor can be cleanly applied.  Can the information content be used to impute the factorization?

*  Wouldn't it be nice to impute a descriptive sparse (e.g. 2-3% of ~10k of activations) version of the 
(say) 500 reals embedding vectors, in such a way that sense-dependencies could lead to improvements
among mutually-related concepts?  
   +  (so embed -> factorize -> re-embed (with factorised structure to boost senses of all words) -> factorize, etc)

But :

*  What would a factorization scheme look like?

*  How could the effectiveness of the factorization be measured?

Looking at the cosine similarity of the (English Wikipedia-derived) embeddings was 
a bit of a let-down : 

*  the ranking wasn't too bad within a cluster near a particular word
*  but the absolute value of the first result didn't bear much relationship 
   to the 'actual similarity' of the cluster.

Perhaps the $l_0$ ideas in the recent paper [Learning Deep $l_0$ Encoders](http://arxiv.org/pdf/1509.00153v1.pdf) could be helpful - 
except that it is really talking about finding the best encoding against a learned dictionary.  
It seems like there is an iterative scheme by which encoders (and their respective weights) can be trained
to converge to the 'ideal' input-to-output dictionary scheme.  

On the one hand, this is not really the 'direction' of learning that I'm thinking about.
On the other, it's encouraging that the dictionary encoding scheme can be discovered automatically.  

Could this scheme be implemented in a more general way - in particular one that could be "Deep" 
(i.e. learns representations at multiple levels)?  Since the 'depth' of the learning is difficult to 
forsee, perhaps some RNN action is in order, so that arbitrary depth structures could be 
imputed by the learning itself (getting deeper as more structure is identified)?

There's also a Google paper on efficiently learning large target output spaces, 
but that seems to involve hashing functions, which don't seem to be adaptive to the space being learned.

[Complete Dictionary Recovery over the Sphere - Sun, Qu &amp; Wright2015](http://arxiv.org/abs/1504.06785) seems like it has relevant results,
as does [Sparse Matrix Factorization - 2014](http://arxiv.org/pdf/1311.3315v3.pdf), where they 
state that (when factorizing $ Y = X_{1}X_{2}X_{3}X_{4} ... $ with $Y$ dense and $X_{i}$ sparse : 

    Our main observation is that one can compute $X_{1}X_{1}^⊤$ by looking at $YY^⊤$ and
    rounding it to an integer. From $X_{1}X_{1}^⊤$ one can recover $X_1$. 
    If $X_1$ has a bounded condition number, it can be inverted and one can solve for 
    $X_{2}X_{3} ...$ and continue like this iteratively to find the rest.

