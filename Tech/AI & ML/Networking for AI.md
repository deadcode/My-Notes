# Explore More
* Interconnect within RACK:
	* Nvidia NVLink
	* UALink (Open standard)
* Scale out level:
	* InfiniBand vs Ethernet
	* Ultra Ethernet Consortium working on UET protocol
* Architectural swift for AI Computing from server based loads to integrated rack scale systems eg. Nvidia GB200 NVL200 platform and AWS Trainium2 Ultraserver.
* Networking vendors include classics Cisco, Juniper, Arista & Nokia along with startups like Arrcus and DriveNets
	* All are exploring new networking technologies like Cell based switching
	* Data Center Interconnect (DCI) is expanding to adopt 400ZR and 800ZR solutions for distributed AI training.
* AI accelerators
	* GPU vs NPU vs LPU (collectively xPU)
* AI Players:
	* OpenAI's ChatGPT (GPT4 family)
	* Google's Gemini
	* Anthropic's Claude
	* xAI's Grok
	* Meta's Llama family
	* Mistral's Le Chat
	* Alibaba's Qwen
	* DeepSeek-R1 (from Chinese hedge fund High-Flyer-backed)
* xPU based data center providers:
	* CoreWeave
	* Lambda
	* Crusoe
* Shift from training time scaling to inference time scaling
	* Investment moving from training to inference time scaling
	* Agentic AI focuses more on test time scaling
* Factors that could reduce investment in AI infrastructure
	* Re-inforcement learning & State Space approaches could improve efficiency compared to today's transformer architecture models
	* Sparsification techniques, exemplified by Neural Magic's (acquired by IBM/RedHat in January 2025) open-source vLLM project, enable inference workloads to run on less powerful hardware, including CPUs
	* Enterprise customers prefer customizing smaller models with proprietary data over using larger FMs
	* ServiceNow Research demonstrates that RAG-enhanced smaller models can outperform larger alternatives
	* NVIDIA's Blackwell GPU announcement in January 2025 promises greater efficiencies with 3-4x performance improvements per watt and dollar. The transition opens opportunities for alternative silicon solutions, including Google's TPUs, AWS Trainium, Azure Maia 100, and offerings from Groq and Cerebras.
* Distributed Data Centers because large FM exceed power capacity of single centers. Building dispersed data centers closer to cheaper power and interconnecting with high speed / low latency optical networks
	* Optical players like Marvell ?
* AI Packages
	* AWS Neuron - AWS Neuron is the software development kit (SDK) used to run deep learning and generative AI workloads on AWS Inferentia- and AWS Trainium-powered Amazon Elastic Compute Cloud (Amazon EC2) instances.
* AI Processors
	* **For complex, large-scale AI**: Consider NVIDIA DGX systems or Google TPU pods
	* **For cost-efficient inference**: AWS Inferentia2 or Google TPU v5e are strong options
	* **For Microsoft ecosystem integration**: Azure's AI offerings provide seamless solutions
	* [Trainium vs Inferentia vs TPU vs GPU](https://www.ankursnewsletter.com/p/google-tpus-vs-aws-trainium-and-inferentia)
* Network design principles: Traditional data center front end networks are arranged in 3 stage Clos pattern (also leaf-spine topology)
# Network scales
* On SoC or die-to-die communication: Open Compute Project (OCP) continues to foster an open chiplet ecosystem through its Open Domain-Specific Architecture (ODSA). Universal Chiplet Interconnect Express (UCIe) is an open industry standard that defines the interconnect between chiplets within a package
* Rack scale or System Level - PCIe vs CXL vs Nvidia NVLink vs UALink
	* (Compute Express Link (CXL) has emerged as a forward-looking open standard for granular connectivity within xPU server clusters and nodes. Built on the PCIe 5.0/6.0 standard, it adds coherence capabilities that allow xPUs to share common memory pools with state synchronization.)
	* NVLink - Nvidia proprietary. The GPUs within each node are interconnected using NVIDIA's proprietary ultra-high-speed, low-latency NVLink technology, which provides exceptional bandwidth and latency characteristics. The 5th-generation NVLink doubled throughput from its predecessor's 900GBps to 1.8 TBps (per GPU). NVIDIA also introduced NVLink Switch, a breakthrough architecture that enables bidirectional full-speed connectivity between every pair of GPU across 576 GPUs
	* UALink (Ultra Accelerator Link) - Open Standard in high-speed, low-latency die-to-die interconnects for AI and HPC accelerators. Still being defined but aims to connect 1024 xPUs. While UALink presents a promising open approach to xPU-to-xPU connectivity with compelling bandwidth per lane performance and scale, NVIDIA maintains several key advantages through NVLink's maturity and deployment experience
* Scale Out/ Row: Ethernet vs UEC UET vs Infiniband - AI networks are split between frontend and backend networks. Frontend connects the xPU cluster to apps and storage typically using multi-tier 100/200 Gbps Ethernet. Backend networks operate at higher 400/800 Gbps speeds.
	* RDMA was developed for Infiniband. RoCE is gaining acceptance and getting popular
	* Scale Out Backend networks:
		* Smart Parallelism Implementation: Training large models require sharding across GPUs. This introduces significant communication overhead between GPUs, making efficient parallelism crucial. Key techniques include:
			* Data Parallelism: Duplicates the entire model on each GPU and splits mini batches. While this requires expensive all-reduce operations for gradients, it's the most straightforward to implement.
			* Pipeline Parallelism: Distributes model layers across GPUs, requiring communication between layers. This reduces memory requirements but needs careful balancing of pipeline stages.
			* Tensor Parallelism: Splits matrix operations across GPUs using all-reduces. This approach minimizes memory usage but requires high-bandwidth, low-latency connections.
			* Multi-Dimensional Parallelism: Combining these techniques becomes necessary when scaling to 10s of thousands of GPUs. For example, Meta's Llama 3 training used all three approaches, creating complex communication patterns with many smaller concurrent communications.
		* Network Optimization Strategies:
			* Network-aware parallelism: Places latency-sensitive techniques like Tensor Parallelism within the same rack where high bandwidth is available, while communication-tolerant techniques like Fully Shared Data Parallelism (FSDP) can span different zones.
			* Topology-aware scheduling: Physical GPU location can impact communication speed. Scheduling systems must understand network topology (rack, row, AI zone) to minimize communication overhead by placing frequently communicating ranks closer together.
			* Advanced scheduling considerations: Must balance multiple objectives including data locality, GPU quotas, fault tolerance, and scheduling overhead. This requires sophisticated algorithms that handle both hard and soft constraints.
			* Control message prioritization: Critical control messages like CTS and ACK receive higher priority in network queues to prevent delays in completion signals and data transfer operations.
			* Spine switch optimization: Tuning Virtual Output Queuing (VOQ) of spine switches reduces their latency impact on data forwarding.
			* Switch architecture considerations: Higher radix switches with shared buffer outputs can address potential bottlenecks in high-density deployments.
			* Enhanced telemetry: Communication libraries must handle multiple concurrent collectives that may be unaware of each other, requiring sophisticated monitoring and routing optimization.
		* Physical Infrastructure Considerations:
			* Power density implications: The increase from traditional 12-15KW per rack to 120KW+ for AI workloads creates new thermal and spacing challenges.
			* Facility adaptation: Data centers without liquid cooling capabilities need increased rack spacing, which directly impacts cable type selection and length requirements.
			* Cable selection strategy: Use passive copper when possible for cost and simplicity, use active cables as a fallback option, and use optical solutions when distance and bandwidth requirements demand it. Topics like the selection of optical modules, Active Optical Cables (AOC), or Direct Attach Cables (DAC), including ACC, AEC, or passive DAC, are important but beyond this report.
	* Scale out - Infiniband: Nvidia aquired Mellanox for Infiniband. RDMA spec 1.8 introduced allows speeds 200Gbps per lane with XDR (extended data record) FEC (forward error correction) with 8 lane QSFP-DD (800Gbps) and OSFP (1600Gbps)
	* Scale Out - Ethernet and RoCE: RoCEv2 interoperability with Infiniband. Most Ethernet fabrics used for AI training support RoCEv2 and implement additional scheduling and load-balancing capabilities. A key differentiator among vendors is their approach to intelligent congestion control for packet loss prevention and latency (including tail latency) reduction.
		* Endpoint and notification-based methods focus on mitigating congestion impact after occurrence. These systems use Priority Flow Control (PFC) where receiving nodes message originating nodes to slow incoming data flows when certain queue-depth thresholds are reached.
		* Multipath-based approaches take a preventive stance through ECMP (Equal Cost Multi-Path), identifying available paths to destinations with identical routing metrics and using hashing mechanisms for data flow load-balancing. While a DPU or SmartNIC can enforce these, they still face challenges with in-cast patterns common in AI training workloads.
		* Scheduling-based solutions represent a more comprehensive approach, preventing congestion through end-to-end flow scheduling from input to output ports. These systems deliver deterministic latency and throughput while eliminating packet loss and reducing jitter. Some vendors have enhanced this further with packet spraying, which distributes traffic at the packet level rather than flow level, providing more granular load balancing than traditional flow-based approaches.
		* Virtual chassis approaches that treat multiple switches as parts of a single logical chassis can help with coordination across the fabric. Networking vendors, including Arista, Arrcus, and DriveNets, have implemented architectures where switches are interconnected to function as a single logical switch or router. These solutions employ centralized control planes to ensure consistent routing, scheduling, and management across all network nodes. They're designed to scale elastically and use advanced scheduling and load-balancing techniques to prevent congestion and optimize utilization.
		* MIT + Meta propose Rail-Only Network that eliminates spine switches (use high bandwidth interconnects within nodes) (Rail = GPUs with the same rank across nodes). On Rail traffic is sparse, non-sparse traffic utilizes NVLink. Promises efficiency and power saving of 33-75%
	* Scale Out - UEC & UET: Ultra Ethernet Transport (UET) protocol, designed to eventually supersede RoCE as the open Ethernet transport protocol for AI and HPC workloads
![[UALink and UET.png]]
* Scale Outside: DCI, Campus, Metro - Optics 400ZR/ZR+, 800ZR/ZR+. Typically employ 2/3 tier Clos Ethernet networks. Growing from 100/200 to 400 Gbps.
	* Scale Out/Outside - Frontend Networks.
		* Security is a concern. Encryption for data in motion, Zero trust, RBAc & Firewalls
		* QoS : IPv6 Segment Routing (SRv6) is getting popular compared to MPLS as it reduces control packet overhead. All vendors support it, esp. Arrcus.
	* Scale Outside - DCI
			* Optical interconnects are major. 400 ZR/ZR+ upto 400km. 800 ZR/ZR+ reached upto 1000km (utilize 16QAM). Major players Marvell, Lumentum & Coherent.
# Opportunity Vectors
## Power Consumption / Optimization
* 20 GW additional power consumption for Data Center in 2025
* 47 GW addition power estimated by Goldman Sachs through 2030*