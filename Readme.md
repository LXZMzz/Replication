# VAReviewer-2024
**This is the Official Implementation of "VAReviewer：Vulnerability Assessment based on LLM with Conflict-Averse Multi-Task Learning"**

For the impact of different learning rates and batch sizes we provide experiment results in `Impacts on learning rate and batch size` folder.

**Environment Setup**:\
pandas\
pytorch\
cvxpy\
scikit-learn\
transformers

**Datasets**:\
Our datasets is based on [DeepCVA](https://github.com/lhmtriet/DeepCVA), and [FVA](https://github.com/Icyrockton/FVA). We also provide it at
[VAReviewer](https://zenodo.org/records/11294639)

After you download and unzip it, you should put the `commit-level.csv` and `function-level.csv` into the corresponding folders and rename it as `codereview_tokens_data.csv`

**Experiments**:\
For RQ 1-3, you can directly run the `main.py` in each folder. 
For example, if you want to run function-level experiments, you can go to function folder and run `main.py`.
In the `constant.py`, you can change the hyperparameter settings or change the model combinations.

For RQ 4. In the `ablation`,you can change the `self.method` in the `constant.py` to mgda, nashmtl and so on to replicate our experiments.

