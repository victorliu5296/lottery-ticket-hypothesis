# lottery-ticket-hypothesis
Standalone python notebook experiment runnable in Google Colaboratory for verifying the [Lottery Ticket Hypothesis paper](https://arxiv.org/abs/1803.03635) using the MNIST dataset 

### Experiment Methodology

The experiment involved training, pruning, and retraining a simple fully connected neural network on the MNIST dataset. The goal was to investigate the effect of iterative pruning on the model's performance and parameter efficiency.

#### 1. **Model Architecture:**
   - The model is a simple feedforward neural network (SimpleFC) with three fully connected layers:
     - `fc1`: Input size of 784 (28x28 flattened MNIST images) and output size of 300.
     - `fc2`: Input size of 300 and output size of 100.
     - `fc3`: Input size of 100 and output size of 10 (number of classes in MNIST).
   - The activation function used after the first two layers is ReLU.

#### 2. **Dataset:**
   - The experiment was conducted on the MNIST dataset, which contains 28x28 grayscale images of handwritten digits (0-9).
   - The dataset was split into a training set and a test set, and each was normalized using the mean and standard deviation of the MNIST dataset.

#### 3. **Training the Baseline Model:**
   - The model was trained for 5 epochs using the Adam optimizer.
   - The negative log-likelihood loss (NLLLoss) was used as the loss function.
   - Training accuracy and loss were tracked across epochs, and the model was evaluated on the test set to record its accuracy and test loss.

#### 4. **Pruning and Retraining:**
   - Pruning was performed iteratively, where 20% of the remaining weights in the fully connected layers were set to zero based on their L1 norm (unstructured pruning).
   - After each pruning iteration, the model was reinitialized and retrained for 5 epochs using the same training procedure.
   - The number of non-zero parameters in the model was recorded after each pruning iteration.
   - Test accuracy, test loss, and confusion matrices were recorded for each pruned and retrained model.

### Experiment Results

| **Iteration** | **Non-Zero Parameters** | **Percentage of Original** | **Test Loss** | **Test Accuracy (%)** | **Training Time (seconds)** |
|---------------|-------------------------|----------------------------|---------------|-----------------------|-----------------------------|
| Baseline      | 266,200                 | 100%                       | 0.0861        | 97.39                 | 132.15                      |
| 1             | 212,960                 | 80.00%                     | 0.0958        | 97.94                 | 132.57                      |
| 2             | 170,368                 | 64.00%                     | 0.1245        | 98.01                 | 132.28                      |
| 3             | 136,294                 | 51.20%                     | 0.1155        | 98.20                 | 134.89                      |
| 4             | 109,035                 | 40.96%                     | 0.1315        | 98.34                 | 146.25                      |
| 5             | 87,228                  | 32.77%                     | 0.1406        | 98.31                 | 147.35                      |


### Key Observations

1. **Pruning Efficiency:** The model retained a high accuracy even as the number of parameters decreased significantly. After the fifth pruning iteration, the model had only 32.77% of its original parameters but still achieved an accuracy of 98.31%, which is slightly higher than the baseline model's accuracy.

2. **Impact on Training Time:** Training time increased slightly with each pruning iteration, potentially due to the higher sparsity of the model, which might have affected the computational efficiency.

3. **Model Performance:** Interestingly, the pruned models sometimes outperformed the baseline in terms of accuracy, suggesting that pruning may have a regularizing effect on the model, preventing overfitting.

4. **Confusion Matrix Analysis:** The confusion matrices generated after each pruning iteration showed that the pruned models were still able to classify the majority of the MNIST digits correctly, indicating the robustness of the pruned models.

### Conclusion

This experiment demonstrates that iterative pruning can significantly reduce the number of parameters in a neural network without a substantial loss in accuracy.
