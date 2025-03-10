# What is it?
LPU stands for Language Processing Unit; it's a proprietary and specialized chip developed by a company called [[GROQ]] (not to be mistaken for the artificial intelligence company Grok headed by Elon Musk). Groq designed LPUs specifically to handle the unique speed and memory demands of LLMs. Namely, an LPU is an especially fast processor designed for computationally intensive applications that are sequential in nature rather than parallel—and LLMs are notably sequential.

Language Processing Units (LPUs) are chips developed to aid in the development of [large language models (LLMs)](https://blog.purestorage.com/purely-informational/the-difference-between-llms-and-mllms/). Built from the ground-up to make language training fast and efficient, LPUs aren’t going to replace GPUs as the preferred chip for AI tasks, but there are likely tasks where they’ll soon be the best possible choice.
# How does it work?
Groq's proprietary LPU is an essential component of its LPU Inference Engine, which is a novel type of processing system. An LPU Inference Engine is a specialized computational environment that addresses compute and memory bandwidth bottlenecks that plague LLMs.

Since an LPU Inference Engine has as much or more compute capacity as a GPU but isn't burdened with external memory bandwidth bottlenecks, an LPU Inference Engine can deliver performance that is measurably orders of magnitude superior to conventional processing systems when training and operating LLMs. That phenomenal throughput has to go somewhere, however, and traditional on-prem data storage solutions can struggle to keep up with an LPU Inference Engine's demands.

LPU Inference Engines operate on a single-core architecture and synchronous networking even across large-scale deployments, and they maintain a high degree of accuracy even at lower precision levels. With excellent sequential performance and near-instant memory access, Groq boasts that the LPU Inference Engine can auto-compile LLMs larger than 50 billion parameters.

Architecturally, LPUs are designed for sequential, rather than parallel, computationally intensive applications. Grok developed LPUs to be inherently efficient and powerful at handling LLMs. The LPU can be deployed in any architecture and can support nearly any training model. With appropriate storage solutions, an LPU can process huge volumes of data and efficiently handle computational demands of training and inference tasks.

# Links
* [What is LPU?](https://www.purestorage.com/knowledge/what-is-lpu.html)
