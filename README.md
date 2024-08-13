# lottery-ticket-hypothesis
Standalone python notebook experiment runnable in Google Colaboratory for verifying the [Lottery Ticket Hypothesis paper](https://arxiv.org/abs/1803.03635) using the MNIST dataset

# Lottery Ticket Hypothesis Experiment Report

## Objective
To test the Lottery Ticket Hypothesis on a simple neural network for MNIST digit classification.

## Method
- Network: Simple fully-connected neural network (784-300-100-10 neurons)
- Dataset: MNIST
- Procedure: Iterative magnitude pruning for 5 iterations
- Pruning: 20% of remaining weights per iteration
- Training: 5 epochs per iteration

## Results

| Model | Accuracy | Parameters | Training Time (s) |
|-------|----------|------------|-------------------|
| Baseline | 97.86% | 266,200 | 148.70 |
| Pruned 80% | 98.05% | 212,960 | 131.09 |
| Pruned 64% | 98.18% | 170,368 | 131.44 |
| Pruned 51.20% | 98.13% | 136,294 | 125.59 |
| Pruned 40.96% | 98.27% | 109,035 | 127.77 |
| Pruned 32.77% | 98.27% | 87,228 | 134.26 |

## Key Observations
1. Pruned networks consistently outperformed the baseline in accuracy.
2. Best accuracy (98.27%) achieved with only 40.96% and 32.77% of original parameters.
3. Training times for pruned networks generally shorter, except for final iteration.
4. Accuracy improved or remained stable despite progressive pruning.

## Conclusion
The experiment supports the Lottery Ticket Hypothesis. Smaller, pruned networks initialized with "winning ticket" weights achieved equal or better performance than the original network, suggesting the existence of efficient subnetworks within larger neural networks.
