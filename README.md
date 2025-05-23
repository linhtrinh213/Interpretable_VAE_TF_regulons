# Interpretable_VAE_TF_regulons
### 📌 Goal 
- This project aims to construct a Interpretable Variational Autoencoder (VAE) that integrates biological networks, specially transcription factor (TF)-gene regulatory network directly into the model structure*.
- This network is incorporated into the model's decoder, in which the TF are the nodes in the latent space, and the genes are the nodes in the next layer. 
- Comparing the learned weights under control and stimulated conditions may give indication on how TF-gene connection change in different conditions ("TF rewiring")
- *: This is a project in the framework of DeepLife4EU course: https://deeplife4eu.github.io/. The data is also provided from here.

### 🧬 Data: 
- The data: peripheral blood mononuclear cells (PBMCs) from systemic lupus patients, either treated with Interferon beta (stimulated) or untreated (control).
- TF-targetd genes network: extracted from collecTRI database: consist of TF and their target genes, including directionality (activation/inhibition).

### 🧠 Model architecture: 
- Encoder: Two-layer fully connected network with dropout.
- Decoder: One-layer sparse linear decoder, structured according to the TF-gene connections. A binary mask matrix enforces sparsity by specifying allowable connections (TF → target gene).


### 🛠️ Method
- From the collecTRI database, a subset of 21 Interferon-related TFs was selected based on literature:
"STAT1", "STAT2", "STAT3", "STAT4", "STAT5A", "STAT5B", "STAT6", "IRF1", "IRF2", "IRF3", "IRF4", "IRF5", "IRF6", "IRF7", "IRF8", "IRF9", "NFKB", "AP1", "MYC", "TP53" 
- Mask construction: define decoder connections
- Filtering of relevant genes: Only genes present in both the dataset and the mask, and with non-zero expression, are retained.
- Two models are trained separately: one for control, one for stimulated conditions.

### 📊 Uncertainties estimation
- Each models is trained multiple times and the mean and standard deviation for the weights are computed.

### 🔄 Stochastic Weight Averaging (SWA): 
- Implementing SWA on our Variational Autoencoder by wrapping Adam optimizer using SWA class, and then train model. After training set the weights of the model to the SWA averages and evaluate the model on test set. 

- SWA works by averaging model weights collected during training with stochastic gradient descent (SGD), its solution tends to produce solutions located at the center of wide, flat regions in the loss landscape, which are known to generalize better. More on the method: https://docs.pytorch.org/docs/stable/optim.html#weight-averaging-swa-and-ema 
