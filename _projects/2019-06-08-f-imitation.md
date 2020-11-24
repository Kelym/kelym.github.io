---
image: 20190608-fimitation-teaser.png
title: Imitation Learning as f-Divergence Minimization
excerpt: We propose a general framework for imitation learning, formulating imitation learning as estimating and minimizing f-Divergence. By plugging in different divergences, we are able to recover existing algorithms such as Behavior Cloning (Kullback-Leibler), GAIL (Jensen Shannon) and Dagger (Total Variation). We address the problem of learning from multi-modal demonstrations. Instead of attempting to learn all modes, we argue that in many tasks it is sufficient to imitate any one of them. We show that the state-of-the-art methods, due to their choice of loss function, often incorrectly interpolate between such modes. Our key insight is to minimize the right divergence between the learner and the expert state-action distributions, namely the reverse KL divergence. Empirical results show that our approximate technique is able to imitate multi-modal behaviors more reliably than GAIL and behavior cloning.
author: <b>Liyiming Ke</b>, Sanjiban Choudhury, Matt Barnes, Wen Sun, Gilwoo Lee, Siddhartha Srinivasa
venue: WAFR
year: 2020
tags: imitation divergence optimization information_theory
arxiv: https://arxiv.org/abs/1905.12888
bibtype: inproceedings
bibname: ke2020imitation
bibauthor: Ke, Liyiming and Choudhury, Sanjiban and Barnes, Matt and Sun, Wen and Lee, Gilwoo and Srinivasa, Siddhartha
bibbook: International Workshop on the Algorithmic Foundations of Robotics
pdf: https://personalrobotics.cs.washington.edu/publications/ke2020fimitation.pdf
---