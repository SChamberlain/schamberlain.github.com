---
name: phylometa-from-r-randomization-via-tip-shuffle
layout: post
title: "Phylometa from R: Randomization via Tip Shuffle"
author: Scott Chamberlain
date: 2011-04-16 13:44:00.004000 -05:00
sourceslug: _posts/2011-04-16-phylometa-from-r-randomization-via-tip-shuffle.md
tags:
- meta-analysis
- Methods
- Evolution
- Phylogenetics
- R
---

---UPDATE: I am now using code formatting from gist.github, so I replaced the old prettyR code (sorry guys). The github way is much easier and prettier. I hope readers like the change.

<a href="http://r-ecology.blogspot.com/2011/04/phylometa-from-r-udpate.html">I wrote earlier</a> about some code I wrote for running Phylometa (software to do phylogenetic meta-analysis) from R.<br /><br />I have been concerned about what exactly is the right penalty for including phylogeny in a meta-analysis. E.g.: AIC is calculated from Q in Phylometa, and Q increases with tree size.<br /><br />So, I wrote some code to shuffle the tips of your tree N number of times, run Phylometa, and extract just the "Phylogenetic MA" part of the output. So, we compare the observed output (without tip shuffling) to the distribution of the tip shuffled output, and we can calculate a P-value from that. The code I wrote simply extracts the pooled effect size for fixed and also random-effects models. But you could change the code to extract whatever you like for the randomization.<br /><br />I think the point of this code is not to estimate your pooled effects, etc., but may be an alternative way to compare traditional to phylogenetic MA where hopefully simply incorporating a tree is not penalizing the meta-analysis so much&nbsp;that you will&nbsp;always accept the traditional MA as better.<br /><br />Get the code <a href="https://gist.github.com/925343#file_phylometa_rand_fxn_one.r">here</a>, and also below. Get the example <a href="http://wp.me/PRT1F-2R">tree file</a> and <a href="http://wp.me/PRT1F-2S">data file</a>, named "phylogeny.txt" and "metadata_2g.txt", respectively below (or use your own data!). You need the file "phylometa_fxn.r" from my website, get <a href="https://gist.github.com/939970">here</a>, but just call it using source as seen below.<br /><br /><script src="https://gist.github.com/925343.js?file=phylometa_rand_fxn_one.R"></script><br /><br />As you can see, the observed values fall well within the distribution of values obtained from shuffling tips. &nbsp;P-values were 0.64 and 0.68 for fixed- and random-effects MA's, respectively. This suggests, to me at least, that the traditional (distribution of tip shuffled analyses, the histograms below) and phylogenetic (red lines) MA's are not&nbsp;different. The way I would use this is as an additional analysis to the actual Phylometa output.

<div class="separator" style="clear: both; text-align: center;"><a href="http://4.bp.blogspot.com/-fpEjXUBvAw8/TanftVw49QI/AAAAAAAAEbY/z9rJKThxRMo/s1600/metadata_2g.txt.jpeg" imageanchor="1" style="clear: left; float: left; margin-bottom: 1em; margin-right: 1em;"><img border="0" height="400" src="http://4.bp.blogspot.com/-fpEjXUBvAw8/TanftVw49QI/AAAAAAAAEbY/z9rJKThxRMo/s400/metadata_2g.txt.jpeg" width="400" /></a></div>
