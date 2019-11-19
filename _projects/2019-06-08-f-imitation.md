---
image: 20190608-fimitation-teaser.png
title: Imitation Learning as $ f $-Divergence Minimization
excerpt: We address the problem of imitation learning with multi-modal demonstrations. Instead of attempting to learn all modes, we argue that in many tasks it is sufficient to imitate any one of them. We show that the state-of-the-art methods such as GAIL and behavior cloning, due to their choice of loss function, often incorrectly interpolate between such modes. Our key insight is to minimize the right divergence between the learner and the expert state-action distributions, namely the reverse KL divergence or I-projection. We propose a general imitation learning framework for estimating and minimizing any f-Divergence. By plugging in different divergences, we are able to recover existing algorithms such as Behavior Cloning (Kullback-Leibler), GAIL (Jensen Shannon) and Dagger (Total Variation). Empirical results show that our approximate I-projection technique is able to imitate multi-modal behaviors more reliably than GAIL and behavior cloning.
author: <b>Liyiming Ke</b>, Matt Barnes, Wen Sun, Gilwoo Lee, Sanjiban Choudhury, Siddhartha Srinivasa
venue: arXiv
year: 2019
tags: imitation divergence optimization information_theory
arxiv: https://arxiv.org/abs/1905.12888
bibtype: article
bibname: ke2019imitation
bibauthor: Ke, Liyiming and Barnes, Matt and Sun, Wen and Lee, Gilwoo and Choudhury, Sanjiban and Srinivasa, Siddhartha
bibbook: arXiv preprint arXiv:1905.12888
---

