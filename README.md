# Towards-graph-foundation-models

[BLOG](https://seemly-sugar-c3d.notion.site/Towards-foundation-model-for-graph-A-Paper-List-1f4e92ce52cb45c7a25b448bb9d2aeff)

This repo is for those models designed for the foundation model in the graph. 


## Path 1: Graph transformers

For a more detailed list of GT-related papers, you may check [awesome graph transformers](https://github.com/wehos/awesome-graph-transformer)

### Rethinking Graph Transformers with Spectral Attention (NIPS 2021) [Paper](https://arxiv.org/pdf/2106.03893.pdf)[Code](https://github.com/DevinKreuzer/SAN)
* SAN, which uses eigenfunctions as positional encodings
### Do Transformers Really Perform Bad for Graph Representation? (NIPS 2021) [Paper](https://arxiv.org/abs/2106.05234) [Code](https://github.com/microsoft/Graphormer)
* Graphormer 
* Pure positional encoding-based designs, use \[VNODE\] as the readout
* Transfer learning experiments first included
### Global Self-Attention as a Replacement for Graph Convolution (EGT) (KDD 2022) [Paper](https://arxiv.org/abs/2108.03348)[Code](https://github.com/shamim-hussain/egt)
* Give a solution to multi-task without clear performance loss -> explicitly learn the embeddings, and add different heads for node/edge/graph-level tasks
### GraphGPS: General Powerful Scalable Graph Transformers (NIPS 2022) [Paper](https://arxiv.org/abs/2205.12454) [Code](https://github.com/rampasek/GraphGPS)
* A blueprint work that tries to revisit and summarize some recent works, which proposes the following framework (I think this is not a pure transformer since local message passing is involved): (1) positional/structural encoding; (2) local message-passing mechanism; (3) global attention mechanism
* What's the difference between PE and SE? PE is more about the global information, like the position in the **whole graph**; SE is more about the local information, like the role in the **local substructure**
* positional/structural encoding: LapPE, RWSE, SignNet, EquivStableLapPE
* local message-passing mechanism: GatedGCN, GINE, PNA
* global attention mechanism: Transformer, Performer, BigBird
### Pure Transformers are Powerful Graph Learners (\*) (NIPS 2022) [Paper](https://arxiv.org/abs/2207.02505) [Code](https://github.com/jw9730/tokengt)
* Pure transformers can work well on graph classification tasks if we augment the original features with node identifiers and edge identifiers, achieving expressiveness better than GNNs
* With proper tokenwise embeddings, transformers can approximate any permutation equivariant linear functions on the graph, which means f(π(x))=π(f(x))
* How to design such embeddings: node-level: \[Xv, Pv, Pv, En\]; edge-level \[Xu,v, Pu, Pv, Ee\]; Pv is the node identifier, En/Ee is the type identifier (whether a node or an edge)
### NAGphormer: A Tokenized Graph Transformer for Node Classification in Large Graphs (ICLR 2023) [Paper](https://openreview.net/forum?id=8KYeilT3Ow)[Code](https://github.com/JHL-HUST/NAGphormer)
* This paper is for **Node classification**
* Hop2Token: for each node v, generate a sequence of size (k, d)-> k is the length of the sequence (k hop), d is the hidden dimension => (X, AX, A2X, ...) => "language"
* An attention-based readout function (attention on k)
* No experiments for semi-supervised setting
### Graph Inductive Biases in Transformers without Message (ICML 2023) Passing [Paper](https://arxiv.org/abs/2305.17589) [Code](https://github.com/liamma/grit)
*  RRWP (Relative Random Walk Probabilities) + MLP can be proven as expressive as Shortest-path distance, polynomial of RR matrices, and adjacency matrices with self-loop
* Similar to EGT, explicitly learn the edge embedding
### A GENERALIZATION OF VIT/MLP-MIXER TO GRAPHS (ICML 2023) [Paper](https://arxiv.org/abs/2212.13350) [Code](https://github.com/XiaoxinHe/Graph-ViT-MLPMixer)
* Patch is the basic for ViT, what's the patch for graph->subgraph generated by METIS
* Encode subgraph with GNN and MLP, no long-range problem since subgraph is small
* Two interesting experiments:
	* Empirical Oversquashing: TreeNeighborMatch
	* CSL, EXP, SR25
### Exphormer: Sparse Transformers for Graphs (ICML 2023) [Paper](https://arxiv.org/abs/2303.06147) [Code](https://github.com/hamed1375/exphormer)
* How to scale graph transformers to large graphs like MPNNs and achieve superior efficiency? GraphGPS has addressed this problem and adopts sparse attention solutions like BigBird or Performer. This paper revisits this problem and proposes a graph-specific solution. 
* Two mechanisms are proposed
	* Virtual global nodes
	* Expander graphs: A type of graph presenting some special theoretical properties, which may preserve the properties of complete graph with only cost O(n)

### ADVECTIVE DIFFUSION TRANSFORMERS FOR TOPOLOGICAL GENERALIZATION IN GRAPH LEARNING (Arxiv 2023) [Paper](https://arxiv.org/pdf/2310.06417.pdf)
* This paper studies structural generalization and proposes a SIGN-like model 
* Strictly speaking, I think the model proposed in this paper can not be viewed as a transformer. It mainly incorporates a learnable global self-attention and combines it with the original adjacency matrix. 
* I like the view in this paper, which views the underlying evolving process of a graph as the heat diffusion and models local message passing with advective diffusion. 

### GRAPHGPT: GRAPH LEARNING WITH GENERATIVE PRE-TRAINED TRANSFORMERS (Arxiv 2023) (\*) [Paper](https://openreview.net/pdf?id=070DFUdNh7)
* Very interesting paper, a must-read!
* This paper demonstrates how to convert a graph into a sequence(Eulerian path), and use pure transformers can achieve very good results on node/edge/graph-level tasks. 
* One missing part is the node feature, which is not well-considered in the current framework. 

### GRAPH CONVOLUTIONS ENRICH THE SELFATTENTION IN TRANSFORMERS! (Arxiv 2023) [Paper](https://openreview.net/pdf?id=poFAoivHQk)
* This paper shows a very simple trick can achieve promising performance to deal with the oversmoothing of transformers
	* Given the self-attention matrix A, view it as the adjacency matrix of a graph, inject higher-order information A^k into the self-attention matrix, and approximate it with Taylor expansion. 
	* The detailed explanation needs further investigation. 






## Path 2: Pre-train graph neural networks 

### STRATEGIES FOR PRE-TRAINING GRAPH NEURAL NETWORKS (ICLR 2020) [Paper](https://arxiv.org/pdf/1905.12265.pdf) [Code](http://snap.stanford.edu/gnn-pretrain)
* Pioneering work in pre-training GNNs in the graph-level tasks, which proposes two pretext tasks
	* context prediction: the subgraph should share similar representations to the context graph
	* attribute masking
* Some observations:
	* 1. Expressiveness of GNNs is related to the effectiveness of pre-training
	* 2. In the experiment, the results show that self-supervised learning is very effective, even more effective than supervised pre-training. Actually, this may be mainly due to the fact that a scaffolding split is adopted. In the NIPS 2022 benchmark paper, similar observation is found on the scaffolding split. 

### GPT-GNN: Generative Pre-Training of Graph Neural Networks (KDD 2020) [Paper](https://arxiv.org/pdf/2006.15437.pdf) [Code](https://github.com/acbull/GPT-GNN)
* Propose masked node attribute generation and masked edge generation 
* To improve performance, the generation is conducted in one round, which means the attribute of some nodes are masked when conducting edge generation 


### Self-Supervised Graph Transformer on Large-Scale Molecular Data (NIPS 2020) [Paper](https://arxiv.org/pdf/2007.02835.pdf) [Code](https://github.com/tencent-ailab/grover)
* An MPNN-involved transformer with randomized layer number 
* Contextual property prediction for node-level SSL
* Graph-level motif prediction for graph-level SSL
* Pretraining a transformer is very expensive, for this GROVER model, needs hundreds of GPUs to pre-train for several days. 


### Graph Meta Learning via Local Subgraphs (NIPS 2020) [Paper](https://proceedings.neurips.cc/paper_files/paper/2020/file/412604be30f701b1b1e3124c252065e6-Paper.pdf)[Code](https://github.com/mims-harvard/G-Meta)
* An extension of MAML in the graph domain, with the prototype (clustering center) and ego subgraph as the bridge
* Three scenarios were considered:
	* Single Graph, Disjoint labels: learn from a set of samples, and test the model on another set of samples with disjoint labels
	* Multiple Graphs, Shared Labels: learn from a set of graphs, and test on the other graphs with a shared label space
	* Multiple Graphs, Disjoint Labels: Hybrid case of 1 and 2

### GPPT: Graph Pre-training and Prompt Tuning to Generalize Graph Neural Networks (KDD 2022) [Paper](https://dl.acm.org/doi/pdf/10.1145/3534678.3539249) [Code](https://github.com/MingChen-Sun/GPPT)
* The setting is quite similar to self-supervised learning, and I think the concept here is closer to aligning the downstream tasks with pre-trained tasks in graph SSL
* Instead of directly doing prediction with the pre-trained embeddings, prompts \[Ttask(y), Tsrt(v)\] is introduced 
* The label is mapped to a continuous prompt, and node classification is viewed as the link prediction task between the label prompt and node prompt
* There's a section in the experiment called prompt without fine-tuning, and I think this prompt strategy is not very useful in that the label information is still used here; Also, it is designed to speedup the training process, which is usually not considered as the efficiency bottleneck of GNNs

### GraphPrompt: Unifying Pre-Training and Downstream Tasks for Graph Neural Networks (WWW 2022) [Paper](https://arxiv.org/pdf/2302.08043.pdf) [Code](https://github.com/Starlien95/GraphPrompt)
* Three phases: pre-training, prompt tuning, and prediction
* The key is to unify the form of tasks across these three stages: basically, we view all as the link prediction tasks => for pre-training, just the EdgePred tasks; for classification task, we get the representation of **each class** as the clustering center of samples from the same class, and then select the class with the shortest distance. 
* One special point of this method is that it can work for different tasks. A special task prompt is inserted and tuned via the prompt tuning to adapt each task. The prompt is viewed as a weight matrix. A special readout function is designed for each task based on the prompt. 


### Does GNN Pretraining Help Molecular Representation? (NIPS 2022) [Paper](https://arxiv.org/pdf/2207.06010.pdf) 
* A benchmark paper that systematically studies the pretraining of GNNs in the molecular domain, with the following conclusion
	* 1. with rich features, self-supervised learning only present very limited or even negative help; supervised fine-tuning is more effective, combing self-supervised & supervised can help
	* 2. pre-training is helpful for data split with distribution split
	* 3. The help from pre-training is more obvious with weaker features

### MOLE-BERT: RETHINKING PRE-TRAINING GRAPH NEURAL NETWORKS FOR MOLECULES (ICLR 2023) [Paper](https://openreview.net/pdf?id=jevY-DtiZTR)[Code](https://github.com/junxia97/Mole-BERT)
* The authors argue that the main bottleneck of AttrMask in GNN-pretraining (ICLR 2020) paper is that the node-level task is too easy. Inspired by VQ-VAE, MoleBERT explicitly learn a discrete codebook, which demonstrates effectiveness. 


### All in One: Multi-Task Prompting for Graph Neural Networks (KDD 2023) [Paper](https://arxiv.org/pdf/2307.01504.pdf)[Code](https://arxiv.org/pdf/2307.01504.pdf)
* Compared to other graph prompt papers, one special point is that prompt is injected to the subgraph (transferring unit) considering both structures and features
* Then, it combines the pretext tasks (like self-supervised learning) with meta-learning, and show good performance. 

### When to Pre-Train Graph Neural Networks? From Data Generation Perspective! (KDD 2023) [Paper](https://arxiv.org/pdf/2303.16458.pdf) [Code](https://github.com/caoyxuan/W2PGNN)
* Using Graphon to model the distribution of pre-trained data, which can be used to
	* determine the portion of knowledge that can be transferrable from the pre-training corpus to the downstream tasks
	* Data pruning
* One main problem: feature is ignored in the theoretical model 

### BETTER WITH LESS: DATA-ACTIVE PRE-TRAINING OF GRAPH NEURAL NETWORKS (NIPS 23) [Paper](https://openreview.net/pdf?id=663Cl-KetJ) 
* This paper argues that by actively selecting the pre-training data, models fine-tuned with downstream tasks can get better performance
* There are mainly two strategies
	* Predictive uncertainty
	* Graph properties (entropy of random walk)

### TOWARDS FOUNDATIONAL MODELS FOR MOLECULAR LEARNING ON LARGE-SCALE MULTI-TASK DATASETS (Arxiv 2023) [Paper](https://arxiv.org/pdf/2310.04292.pdf) [Code](https://github.com/datamol-io/graphium)
* the largest benchmark dataset for molecular pre-training
* interestingly, in many cases, multi-dataset training present no gain, which somehow doubt the effectiveness of using GNNs as the blocks of graph foundation models


## Path3: Language model pre-training (co-training & co-finetuning)

### DeepWalk, Node2Vec, PTE 
* Traditional walk-based pre-training
* Equivalent to matrix factorization of a matrix such as normalized graph laplacian
* do not consider node features

### LinkBERT: Pretraining Language Models with Document Links (ACL 2022)[Paper](https://arxiv.org/pdf/2203.15827.pdf)[Code](https://github.com/michiyasunaga/LinkBERT)
* On the basis of BERT, consider the relationship among different documents, which use the following two tasks
	* Masked language modeling
	* Document relation prediction: Segment documents into chunks. Given two chunks, classify their relationship into contiguous, random, or linked. 


### NODE FEATURE EXTRACTION BY SELF-SUPERVISED MULTI-SCALE NEIGHBORHOOD PREDICTION (ICLR 2022) [Paper](https://arxiv.org/pdf/2111.00064.pdf) [Code](https://github.com/amzn/pecos/tree/mainline/examples/giant-xrt)
* View the structural fine-tuning of LMs as an extreme multilabel classification (XMC) problem, which means for graph: A-B, A-C, C-D, the label for node A is \[1, 1, 1, 0]. 

###  Learning on Large-scale Text-attributed Graphs via Variational Inference (ICLR 2023)  [Paper](https://arxiv.org/abs/2210.14709) [Code](https://github.com/AndyJZhao/GLEM)
* Co-train an LM and a GNN iteratively using the EM framework 


### Graph-Aware Language Model Pre-Training on a Large Graph Corpus Can Help Multiple Graph Applications (KDD 2023) [Paper](https://arxiv.org/pdf/2306.02592.pdf) 
* GaLM mainly involves the following steps
	* Pretraining-stage:
		* 1. Graph-aware LM pre-training: tune the parameters of a Bert model with unsupervised pretext task: link prediction
		* 2. Observation: co-training is not very useful
	* Fine-tuning stage:
		* 1. Graph-aware LM fine-tuning on various applications
		* 2. stitching large graph corpus at the data level

### Walklm: a uniform language model fine-tuning framework for attributed graph embedding (NIPS 2023)
* wait for the camera-ready version




## Path 4: Graph as Natural Language + LLMs
Here for natural language, we consider both
* directly use human language, this usually can only be applied to the most powerful LLMs like ChatGPT and GPT4
* use tokens from LLMs' dictionaries, or use prompt tuning to align the feature space of LLMs and graphs 


### Can Language Models Solve Graph Problems in Natural Language? (NIPS 2023) [Paper](https://arxiv.org/abs/2305.10037) [Code](https://github.com/arthur-heng/nlgraph)
* Use natural language to directly describe the graph structure, and propose the NLGraph benchmark for algorithmic reasoning on the graph.
* Language: Human language



### Natural Language is All a Graph Needs (Arxiv 2023) [Paper](https://arxiv.org/abs/2308.07134)
* Tuning T5 for node classification tasks, using link prediction as the auxiliary tasks
* Language: Human language with node features inserted (those features are added to the vocabulary sets of the LLMs)

### Graph Neural Prompting with Large Language Models (Arxiv 2023)
* Graph language here: embedding of the encoded graphs + text embeddings (from the LLM dictionary) of the entities => used as the prompt 
* auxiliary link prediction tasks
* Language: Continuous prompt generated by GNNs

### GRAPHTEXT: GRAPH REASONING IN TEXT SPACE (Arxiv 2023)
* Generate a graph syntax tree
* A first-order traversal of the GST can generate a sequence of required information, which can be further adopted as the prompt
* Propose a novel graph learning paradigm: interactive graph reasoning => Use human reminders to guide the correct reasoning steps
* Language: prompts induced by the graph syntax tree


### GRAPHLLM: BOOSTING GRAPH REASONING ABILITY OF LARGE LANGUAGE MODEL (Arxiv 2023) [Paper](https://arxiv.org/pdf/2310.05845.pdf)[Code](https://github.com/mistyreed63849/Graph-LLM)
* Add structure-aware prefix prompts to inputs
* How to generate those structure-aware prefix prompts: first use a transformer encoder to generate a context vector, then use the decoder to generate the node features, which are then processed by a graph transformer to generate the final embedding (this design is related to the nature of dataset in this paper, since they don't present much textual information )
* Language: prefix prompt 


### TALK LIKE A GRAPH: ENCODING GRAPHS FOR LARGE LANGUAGE MODELS (Arxiv 2023) [Paper](https://arxiv.org/pdf/2310.04560.pdf)
* Very similar to the NLGraph benchmark, also use the natural language to describe the graph structures
* Language: human language


### IN-CONTEXT LEARNING FOR FEW-SHOT MOLECULAR PROPERTY PREDICTION (Arxiv 2023) [Paper](https://arxiv.org/pdf/2310.08863v1.pdf)
* Demonstrate one meta-learning-inspired way to do few-shot prediction on large transformers without fine-tuning
* Encode query, and support points together with their labels, and then concatenate them together into embeddings. Embeddings are further adopted as a prompt and inserted into the transformer. 


## Path 5: Unifying data in a large graph
### PRODIGY: Enabling In-context Learning Over Graphs
* GNN-based multi-task multi-dataset setting
* Show the knowledge between MAG and WIKI can be transferred 
* Manually construct a large heterogeneous graph that consists of different types of nodes; use these nodes as guidance to transferring

### ONE FOR ALL: TOWARDS TRAINING ONE GRAPH MODEL FOR ALL CLASSIFICATION TASKS
* How do we align the feature space? Use sentence-bert to encode all, the attribute is required(this method can not directly work for continuous features, what they do is using a prompt to describe the attribute)
* How to do multiple datasets? 
	* NOI prompt node->necessary subgraphs for node/link/graph-level tasks
	* Class nodes->Give the information of classes so zero-shot learning is possible


## Path 6: Diffusion Models & S4
### Data-Centric Learning from Unlabeled Graphs with Diffusion Model (NIPS 2023) [Paper](https://arxiv.org/pdf/2303.10108.pdf)
* Diffusion model
### S4G: BREAKING THE BOTTLENECK ON GRAPHS WITH STRUCTURED STATE SPACES （Arxiv 2023) [Paper](https://openreview.net/pdf?id=0Z6lN4GYrO)
* Adopting S4 to handle long-range information and over-squashing problem






## Others

### Foundation models for KG

### TOWARDS FOUNDATION MODELS FOR KNOWLEDGE GRAPH REASONING [Paper](https://arxiv.org/pdf/2310.04562.pdf)
