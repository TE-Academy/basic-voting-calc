# basic-voting-calc
A basic voting calculator for use in the TE Academy Reputation-Weighted-Voting course. 

## Overview

This repository has two purposes:
:bulb: **Specific Purpose.** It has specific information about the design chosen for use in the TokenEngineering Academy Study Season fellowship, in conjunction with the TokenEngineering Academy **Reputation Weighting Voting** Course.
:bulb: **General Purpose.** It offers a lightweight general framework for experimenting with voting mechanisms. 

## TE Academy Study Season Fellowship Mechanism

At its core, a weighted voting mechanism must do four things to select a single winner:
**Weighting Step.** Assign weights to individual wallets, based on some  about available information the voters and their credentials. 
**Voting Step.** Allow voters to express preferences for individual candidates, by signals on *ballots*. 
**Counting Step.** Combine the information from the ballots into a single winner. 

For the mechanism used in the TE Academy Study Season Fellowship decision, the Voting and Counting steps are relatively straightforward. Most of the logic is in processing voter information into weights. 

### Weighting Step

This step is performed in a few different functions in [`utils.processing`](https://github.com/TE-Academy/basic-voting-calc/blob/main/utils/processing.py)

The primary function `create_dict_for_equal_cweights` assigns weights to the available NFTs, so that the fundamental property
$ \text{expert credential weight} \geq \text{graduate credential weight} \geq \text{student credential weight}$
is satisfied. This involves taking an initial weighting dictionary, and re-scaling the weights of specific NFTs so that this property holds. 

All other functions in the module are helper functions, doing data validation or preprocessing to assure that this property is met. 

Once the final recalculated weights have been assigned to each NFT, each individual wallet is given a weight by summing the weights of all NFTs held by that wallet. 

### Voting and Counting Steps

The logic for the voting process itself is held in [`mechanisms.percentage_allocation_weighted_percentage`](https://github.com/TE-Academy/basic-voting-calc/blob/main/mechanisms/percentage_allocation_weighted_plurality.py)`. 

For the voting process itself, the process is relatively straightforward. 

Each wallet selects a percentage to express for each candidate. This percentage is multiplied by the voter's weight to give the *weighted vote* for each candidate. 

For the final processing step, we add all weighted votes for each candidate together, yielding a final "weighted votes" number for each candidate. The winner is whichever candidate has the most weighted votes after this step.

## Simulations

There are simulations available to give more information about the mechanism, and important properties related to its use. 

The primary simulation for this mechanism is available in [this Jupyter notebook.](https://github.com/TE-Academy/basic-voting-calc/blob/main/experiments/experiment_2b-reweighting-june-19-data.ipynb)

For comparison's sake, there is also a [notebook](https://github.com/TE-Academy/basic-voting-calc/blob/main/experiments/experiment_1b-default-weights-june-19-data.ipynb) for a simpler variation that does not re-scale any of the original weights, which was used for comparison in making the decision. 
