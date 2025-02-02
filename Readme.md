------

# VAReviewer-2025

**This is the Official Implementation of "Towards Task-Harmonious Vulnerability Assessment based on LLM"** 

------

## **Abstract**

Software vulnerabilities seriously jeopardize software security. It would be highly beneficial if developers could receive severity reminders regarding vulnerabilities when developing software systems. Therefore, when handling numerous vulnerabilities, it's crucial to prioritize the most critical ones and assess their severity early for effective resolution. Vulnerability assessment needs to train multiple assessment tasks simultaneously. Previous works suffer from task-disharmonious issues when conducting vulnerability assessments because they fail to balance the magnitude of gradients across multiple tasks and the conflicts in gradient directions. Additionally, they use identical code embedding for all classifiers without extracting task-related features.

In this study, we are the **first to conduct vulnerability assessment in a task-harmonious way** by harmonizing gradient direction and magnitude, and filtering out task-specific features for each classifier. In addition, we use finer-grained contextual information than existing works by program slicing to further boost the model performance. Through extensive experiments, our model achieves state-of-the-art performance at both the commit and function levels. Specifically:

- **Function-level tasks**: Our model achieves an average **F1-Score of 0.819** and **MCC of 0.742**, outperforming all baseline models.
- **Commit-level tasks**: Our model improves the best baseline model’s average performance by **29.6%** in F1-Score and **64.7%** in MCC.

------

## **Experiment Results**

For the impact of different learning rates and batch sizes we provide experiment results in `Impacts on learning rate and batch size` folder.

------

## **File Structure and Explanation**

The following directories — `ablation`, `commit`, and `function` — each contain **6 files**. Below is a detailed explanation of each file:

1. **`Constant.py`**:
   - This file contains various hyperparameters for the experiment. You can modify the following settings:
     - **Learning rate**
     - **Epochs**
     - **Random seed**
     - **CUDA or CPU configuration**
     - **Whether to use VIB (Variational Information Bottleneck)**
     - **Selection of multi-task learning optimizers (e.g., MGDA, NashMTL)**
   - This file helps to set up the environment and control key experiment variables.
2. **`main.py`**:
   - The startup file for the project. It includes the process for:
     - **Dataset loading**
     - **Model loading**
     - **Training the model**
     - **Evaluating the model**
   - This file orchestrates the overall execution of the model and ties together all the components.
3. **`review.py`**:
   - In this file, we construct the entire **VAReviewer** model. It includes the model architecture, forward pass, and other important components needed for vulnerability assessment.
4. **`VIB.py`**:
   - This file constructs the **VIB classifiers**. VIB stands for **Variational Information Bottleneck**, a technique used to improve model performance by mitigating overfitting and boosting generalization.
   - It handles classifier construction and the interaction with the core VAReviewer model.
5. **`weight_methods.py`**:
   - This file contains implementations for various multi-task learning optimizers, such as:
     - **UW** (Uncertainty Weighting)
     - **NashMTL**
     - **MGDA** (Multi-Gradient Descent Ascent)
     - **FAMO** (Feature-Aware Multi-task Optimization)
   - These methods are crucial for balancing multiple tasks in multi-task learning scenarios and improving performance in vulnerability assessment.
6. **`min_norm_solvers.py`**:
   - This file supports `weight_methods.py` by optimizing **multi-task learning models** to find the optimal combination of task weights or solutions that minimize the overall norm. This is crucial for balancing gradients and improving convergence during training.

------

## **Environment Setup**

To run the project, make sure you have the following dependencies installed:

- **pandas**: For data manipulation and analysis.
- **pytorch**: For deep learning and model training.
- **cvxpy**: For optimization tasks.
- **scikit-learn**: For machine learning algorithms and metrics.
- **transformers**: For transformer-based model architecture (e.g., BERT, GPT).

You can install these dependencies via pip:

```bash
pip install pandas torch cvxpy scikit-learn transformers
```

------

## **Datasets**

The datasets used in this project are based on **[DeepCVA](https://github.com/lhmtriet/DeepCVA)** and **[FVA](https://github.com/Icyrockton/FVA)**. You can access the datasets here:

- **VAReviewer Dataset**: [VAReviewer Dataset on Zenodo](https://zenodo.org/records/11294639)

Once you download and unzip the datasets, make sure to place the following files in the appropriate folders(eg.`commit-level.csv` to the commit folders) and rename them as `codereview_tokens_data.csv`:

- `commit-level.csv`
- `function-level.csv`

------

## **Running Experiments**

### For RQ1-RQ3 (Commit-level and Function-level Tasks):

1. **Commit-level experiments**:

   - Go to the `commit` folder and run the following command:

     ```bash
     python main.py
     ```

   - You can modify hyperparameters and model settings in `Constant.py`.

2. **Function-level experiments**:

   - Similarly, go to the `function` folder and run the following command:
     ```bash
     python main.py
     ```

3. **For RQ3**:

   - In the `constant.py`, you can change the hyperparameter settings or change the model combinations. 
   
     ①`Soley Fully connected layers as classifers`： `self.VIB = False`, `self.mtl = False` and then change self.ablation to function or commit to test the model performance at different level.
   
     ②`Soley VIBs `: `self.VIB = True`, `self.mtl = False` 
     
     ③`VIBs with balance graident direction `: `self.VIB = True`, `self.mtl = True` ,`self.method = 'GD'`
     
     ④`VIBs with balance graident magnitude `: `self.VIB = True`, `self.mtl = True` ,`self.method = 'GM'`
     
     ⑥`VAReviewer `: `self.VIB = True`, `self.mtl = True` 

------

### For RQ4:

   - Firstly go to the `ablation` folders. In `constant.py`, you can change the hyperparameters like `self.ablation`, and `self.method` to test the model performance in different level tasks with different multi-task learning optimizers.
   - For example change self.ablation = 'commit' and self.method='mgda', you can test the model performance with MGDA in commit level tasks.

   - This allows you to replicate the ablation experiments discussed in the paper and compare different methods.

------

## **Conclusion**

This repository provides the official implementation of **VAReviewer-2024**, which proposes a novel task-harmonious approach for vulnerability assessment based on large language models (LLMs). By using fine-grained **program slicing**, harmonizing gradients, and filtering task-specific features, we have demonstrated significant improvements in both **commit-level** and **function-level** vulnerability detection tasks. We hope this implementation helps advance research in software security and vulnerability assessment.



**Let me know if you need further adjustments!**
