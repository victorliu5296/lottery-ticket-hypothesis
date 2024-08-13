# lottery-ticket-hypothesis
Standalone python notebook experiment runnable in Google Colaboratory for verifying the [Lottery Ticket Hypothesis paper](https://arxiv.org/abs/1803.03635) using the MNIST dataset

**Disclaimer: this experiment is probably done wrong for now, I'll work on fixing it iteratively.** 

### Experiment Methodology

The experiment involved training, pruning, and retraining a simple fully connected neural network on the MNIST dataset. The goal was to investigate the effect of iterative pruning on the model's performance and parameter efficiency.

#### Methodology

1. **Model Architecture:**
   - A simple fully connected network with three layers: 
     - `fc1`: 784 → 300
     - `fc2`: 300 → 100
     - `fc3`: 100 → 10
   - ReLU activations after `fc1` and `fc2`.

2. **Dataset:**
   - MNIST, with 28x28 grayscale images of digits (0-9).
   - Data normalized and split into training and test sets.

3. **Training:**
   - Baseline model trained for 5 epochs with Adam optimizer and NLLLoss.
   - Performance: 97.39% accuracy, 0.0861 test loss, 266,200 non-zero parameters.

4. **Pruning and Retraining:**
   - Iterative pruning of 20% of remaining weights based on L1 norm.
   - Model reinitialized and retrained for 5 epochs after each pruning iteration.
   - Non-zero parameters, test accuracy, and test loss recorded after each iteration.

### Experiment Results

| **Iteration** | **Non-Zero Parameters** | **Percentage of Original** | **Test Loss** | **Test Accuracy (%)** | **Training Time (seconds)** |
|---------------|-------------------------|----------------------------|---------------|-----------------------|-----------------------------|
| Baseline      | 266,200                 | 100%                       | 0.0861        | 97.39                 | 132.15                      |
| 1             | 212,960                 | 80.00%                     | 0.0958        | 97.94                 | 132.57                      |
| 2             | 170,368                 | 64.00%                     | 0.1245        | 98.01                 | 132.28                      |
| 3             | 136,294                 | 51.20%                     | 0.1155        | 98.20                 | 134.89                      |
| 4             | 109,035                 | 40.96%                     | 0.1315        | 98.34                 | 146.25                      |
| 5             | 87,228                  | 32.77%                     | 0.1406        | 98.31                 | 147.35                      |


### Observations

For pruning, the PyTorch utility module was used. So the architecture wasn't modified, but the parameters were set to 0. I find it strange that the training time would actually increase for the same amount of epochs. As the model got smaller, it seems like the test loss increased while test accuracy also increased. Training time also increased. 

### Conclusion

Using this recipe, we are able to reduce model size during training, which is often very expensive. This is cheaper when properly optimized for since you have less parameters, and therefore less calculations. Especially when you reduce the size to, say, 20%, this impact is significant on efficiency without stumping performance.
