---
tags:
  - università/datacenter-design-and-operation
  - cloud-computing
  - service-models
  - deployment-models
data: 2026-05-14
lezione: "17 - Cloud Computing"
professore: "Antonio Cisternino"
---
# Cloud Computing

Cloud computing is one of the central topics of this course — not only because it directly concerns modern datacenter architecture, but because it has redefined the way organizations think about IT. The key point the professor starts from: cloud was not born as a technology, but as a **business model**.

---

## Origins and Historical Context

In the early 2000s, Amazon, Microsoft, and Google were building increasingly large datacenters to support seasonal traffic peaks (Christmas, summer). Outside those peaks, servers sat idle. Someone — Amazon in particular — had the idea of monetizing that excess capacity: why not rent it out to third parties?

This marks the birth of cloud: not as a technical invention, but as a response to an economic problem. Google had already offered something similar with BigTable — the ability to pay to index data using Google's search infrastructure — but Amazon found the right recipe, the one that became the AWS we know today.

The deepest change was not technological but financial: the shift from **CapEx** (Capital Expenses) to **OpEx** (Operational Expenses). Previously, an organization bought servers it owned; now it pays for a service, and if it stops paying it is left with nothing. This is a massive paradigm shift.

> [!tip] The CapEx vs OpEx logic
>
> With CapEx you buy something you own and depreciate over time. With OpEx you pay for use: if you stop using it, you stop paying. Cloud shifted IT spending toward OpEx, eliminating the upfront investment but introducing a continuous dependency on the provider.

In the first cloud wave, many Chief Information Officers thought they could completely get rid of on-premise IT. The promise was attractive: eliminate the entire IT department by delegating everything to the cloud. Then the problems came:

- **Data sovereignty**: using US cloud means the American government can access your data. The NSA was even spying on Angela Merkel; Germany reacted by requiring Microsoft to create an independent German subsidiary to manage its services on German soil.
- **Bandwidth and latency**: an MRI scan can produce 7 TB in half an hour. How do you upload that to the cloud? You can't. With the rise of IoT, controlling a traffic light via a cloud service introduces unacceptable latency.
- **New workloads**: video, content, documents are suitable for cloud; real-time control systems are not.

These limitations scaled back the promise of "full cloud", but cloud remained fundamental for many workloads.

---

## The Formal NIST Definition

The **National Institute of Standards and Technology** (NIST) provided the most widely used and still valid definition of cloud computing:

> [!definition] Cloud Computing (NIST)
>
> Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (networks, servers, storage, applications, services) that can be rapidly provisioned and released with minimal management effort or service provider interaction.

The name "cloud" is evocative: you have no idea where your resources are physically running, just as you don't know where a cloud is physically located in the sky. This is made possible by virtualization and live migration: a service can be physically moved from one continent to another without the user noticing.

---

## The 5 Essential Cloud Characteristics

NIST identifies five characteristics that must be present for a service to be defined as cloud.

### On-demand self-service

A consumer can unilaterally provision computing capabilities without requiring human interaction with the service provider. You go to a web portal, select resources, enter a credit card, and get virtual machines in minutes. On AWS, for example, there is no manual approval process.

### Broad network access

Capabilities are available over the network and accessible through standard mechanisms that support thin and thick clients: phones, tablets, laptops, workstations. The critical corollary of this characteristic is that **without a network you have access to nothing**: cloud is useless offline. So much so that some apps advertise offline functionality as a premium feature.

### Resource pooling

The provider's computing resources are pooled to serve multiple consumers using a **multi-tenant** model. Physical and virtual resources are dynamically assigned and reassigned according to demand.

The concept of **tenant** is fundamental: a tenant is the set of resources used by an organization within the cloud. A multi-tenant cloud simultaneously hosts the University of Pisa, the University of Bologna, and so on, each with its own security policies and identities.

An important aspect is **location independence**: the customer generally has no control or knowledge of the exact location of their resources, but may specify location at a high level (country, region). For public administrations in Italy, it is legally mandatory to guarantee European data residency.

### Rapid elasticity

Capabilities can be elastically provisioned and released — in some cases automatically — to scale rapidly outward and inward commensurate with demand. To the consumer, resources appear *infinite*: the datacenter is large enough that there is almost always room for one more virtual machine.

> [!example] Elasticity: the video game example
>
> Ubisoft does not own a datacenter sized for the peak of a game launch. In the launch weeks there is an enormous ramp-up in demand, then traffic drops. If Ubisoft bought hardware for the peak, it would sit idle almost all the time. With cloud, resources are added during the peak, paid for, and released when traffic subsides.

> [!example] Automatic elasticity: the tax filing system
>
> Italy's tax filing system has a massive peak on the deadline day (everyone waits until the last moment). With automatic elasticity you can configure: "if the web server response latency exceeds X, add a worker; if it drops below Y, remove one." The system scales itself, paying only for the resources actually used.

It is important to distinguish **scale** (total infrastructure size) from **elasticity** (the ability to allocate and deallocate rapidly on short timescales). Buying a server and selling it after 5 years is not elasticity; requesting 1000 CPUs and getting them in 5 minutes, then releasing them in an hour, is.

### Measured service

Cloud systems automatically control and optimize resource use through metering capabilities. Resource usage can be monitored, controlled, and reported, providing transparency for both provider and consumer.

Measurement happens at a level of abstraction: the provider does not tell you which specific SSD holds your data, but offers service tiers — bronze, silver, gold — each with different throughput guarantees (e.g., bronze: 500 MB/s, silver: 1 GB/s, gold: 5 GB/s). You pay for the tier, not the specific device.

![[dcdo_cloud_nist_definition.png]]
*Fig. — The five essential characteristics of cloud computing according to NIST.*

---

## Cloud Computing Benefits

Cloud brings several advantages over the traditional on-premise model.

**Business agility**: the ability to respond quickly to market opportunities without waiting months to purchase, install, and configure hardware.

**Reduced IT costs**: this is the most advertised promise, but also the most insidious. Material costs (hardware) are easy to measure. Software costs are not: the price of software is whatever the market is willing to pay, not what it costs to produce. HCI (Hyper-Converged Infrastructure) licenses have become so expensive that many organizations are returning to traditional networked storage. Warning: you can easily end up paying more than before.

**High availability**: a cloud provider with dozens of global datacenters can offer redundancy and resilience unreachable by a single organization. If one datacenter is hit (for example by a missile attack, as mentioned in the example), traffic is redirected elsewhere.

**Flexible scaling**: the ability to dynamically scale both horizontally (scale-out) and vertically (scale-up).

**Development simplification**: organizations used to maintain double the infrastructure — one for production, one for testing. With cloud, you provision a test environment, use it, release it.

---

## Drawbacks and Limitations

Cloud drawbacks are real and practically relevant.

The **data sovereignty** problem is still open. The US government claims the right to access all data stored on US-owned cloud, regardless of physical location. Microsoft, Google, and others have taken legal action to oppose this, with partial success. Germany solved the problem by requiring Microsoft to create an independent German subsidiary to manage German data.

**Vendor lock-in** is structural: if you build your entire infrastructure on a single cloud provider's proprietary APIs, switching later is extremely costly. Diversifying investments and using open APIs mitigate but do not eliminate the problem.

**Software licensing** is complex territory: there are professionals paid exclusively to know the details of Microsoft licensing contracts. A change in license terms can completely overturn the economic rationale for a technology choice.

---

## Cloud Service Models

NIST formalizes three main service models that are essential to know. The professor uses the pizza analogy to make them immediately memorable.

> [!tip] The pizza analogy
>
> Think of pizza as everything needed to eat: ingredients, dough, cooking, oven, gas/electricity, table, drink.
> - **On-premise**: you do everything at home (you manage everything).
> - **IaaS**: you buy a frozen pizza at the supermarket and bake it at home (the provider manages up to the OS, you manage the rest).
> - **PaaS**: you order home delivery (the provider manages everything except application customization).
> - **SaaS**: you go to the restaurant (the provider manages everything, you only use the final service).

### Infrastructure as a Service (IaaS)

The provider offers raw computational capability: processors, storage, networks. The consumer can deploy and run any software, including operating systems and applications. The consumer **does not manage** the underlying cloud infrastructure, but **has control** over OS, storage, deployed applications, and — to a limited extent — networking components (e.g., firewall).

In practice: the provider installs the hardware and delivers a "blank" OS; you configure, update, and manage the entire software stack above it.

![[dcdo_cloud_iaas_stack.png]]
*Fig. — In IaaS the provider manages hardware and virtualization; the consumer manages from OS upward.*

### Platform as a Service (PaaS)

The consumer deploys their own applications on cloud infrastructure using languages, libraries, and tools supported by the provider. They do not manage or even know the underlying OS — that is the provider's responsibility. Control is limited to the application only.

**Practical examples**: WordPress is a typical PaaS. Want a website? You don't worry about the web server, PHP, or operating system. You buy a WordPress instance, customize it with your own plugins and themes, and the provider manages everything else. Salesforce is another example on the commercial side.

![[dcdo_cloud_paas_stack.png]]
*Fig. — In PaaS the provider manages up to the runtime; the consumer manages only the application.*

### Software as a Service (SaaS)

The consumer directly uses the provider's application running on cloud infrastructure. They manage nothing of the infrastructure, platform, or application itself — only user-level configuration.

**Examples**: Gmail is SaaS via browser. Microsoft 365 is SaaS with locally downloadable applications but intrinsically tied to the cloud: if you stop paying, the apps stop working after 30 days.

![[dcdo_cloud_saas_stack.png]]
*Fig. — In SaaS everything is managed by the provider; the consumer only uses the final product.*

### Cloud Services Brokerage (CSB)

**Brokerage** is an intermediate layer that tries to allocate cloud resources on the most cost-effective provider among those available (AWS, Azure, GCP…). Interesting in theory; in practice, once Microsoft and others adopted the strategy of automatically matching competitor prices, the economic advantage of brokers nearly disappeared.

---

## Cloud Deployment Models

Beyond service models, there are *deployment* models that define who can access the infrastructure.

### Public cloud

The infrastructure is provisioned for open public use. Anyone can create an account on AWS, Azure, GCP, Oracle Cloud and allocate resources, without restrictions. "Public" does not mean free: it means accessible to everyone.

### Private cloud

The infrastructure is dedicated exclusively to a single organization. It may be managed by the organization itself or by third parties, and may exist on-premise or off-premise. The University of Pisa, for example, operates its own private cloud.

The key point: a private cloud is not private because it is *owned* by the organization, but because only that organization can allocate resources on it.

### Community cloud

Infrastructure shared among organizations with a common mission or requirements (e.g., Italian universities, European public administrations). May be on-premise or managed by third parties.

### Hybrid cloud

A combination of two or more distinct cloud infrastructures (private, community, public) that remain unique entities but are bound together by standardized technology enabling data and application portability.

> [!example] UniPi as a hybrid cloud
>
> When you access a University of Pisa service, you are probably using three different clouds without knowing it: teaching services run in the San Piero datacenter (UniPi private cloud), Alunni runs on the Cineca datacenter in Bologna, and documents are on OneDrive (Microsoft Azure). Everything appears as a single experience with a single identity. This is hybrid cloud in practice.

![[dcdo_cloud_hybrid_model.png]]
*Fig. — The hybrid cloud model combines different cloud infrastructures linked by open standards.*

> [!note] Impact of private cloud on IT organization
>
> Cloud — even in its private form — revolutionized the organizational model of IT. Previously each project bought its own server; unused capacity was pure waste. With private cloud, a shared pool is managed from which each project draws the resources it needs, returning them when no longer required. Technologies like VMware vCloud Foundation, Microsoft Azure Local (formerly Azure Stack), and Microsoft System Center enable this model on-premise.

---

## The Cloud Computing Reference Model

Once cloud architectures converge toward common patterns, it makes sense to define a **reference model** — a shared map describing how a cloud infrastructure is organized. The OASIS model (referenced by NIST) remains valid after more than 12 years.

![[dcdo_cloud_reference_model.png]]
*Fig. — The OASIS cloud computing reference model: functional layers and cross-cutting aspects.*

The model is structured around vertical functional layers and horizontal cross-cutting aspects.

### Physical layer

At the base there is always silicon: compute, storage, network — everything covered in this course. Most cloud is implemented via virtualization, but **bare metal cloud** also exists: instead of a VM, you provision an entire physical server by programming it via API (e.g., Redfish). Less granularity in elasticity (whole servers only), but useful for HPC.

### Virtual layer

The crucial layer: it abstracts physical resources and presents them as a homogeneous pool from which to allocate slices. Virtual machines, virtual storage, virtual networks (VLAN, vSAN) live here. Virtualization is what makes resource pooling possible.

### Control layer

The API that lets you say "give me a VM with these characteristics" and get the result. The control layer knows where free resources are, marks them as in use, and keeps the ledger. It is the operational "brain" of the cloud: when a request comes in, the control layer queries the inventory, finds available capacity, allocates, and confirms.

### Administration / Orchestration layer

Above the control layer sits automation: workflows that define "if X happens, do Y, then Z." It handles complex provisioning (allocate a VM, wait for it to be ready, configure it, notify the user), hardware decommissioning (when a server is retired: disk wipe, removal from the configuration database, notification), and everything that would otherwise require human intervention.

> [!tip] The economics of automation
>
> Large cloud providers manage tens of millions of servers with very few physical staff (a few dozen people for enormous facilities). This is only possible because the orchestration layer automates virtually everything: from replacing a failed disk to replacing an entire rack.

### Service layer

The topmost layer: the web portal where the user sees the service catalog, authenticates, selects resources, pays, and receives them. It translates user requests into tasks that flow down to the orchestration and then the control layer.

### Cross-cutting aspects

Security and business continuity do not belong to a single layer: they permeate all of them. Security must be present at the physical level (physical access to the datacenter), virtual level (tenant isolation), control level (API authentication), orchestration, and service. Business continuity — RAID, backup, replication, inter-datacenter failover — is equally cross-cutting.

---

## Greenfield vs. Brownfield

Two fundamental terms in IT jargon:

**Greenfield**: you start from scratch. New hardware, system deployed from the ground up, clean tests, delivery. The ideal case.

**Brownfield**: you deploy something into an existing infrastructure. Decisions made 10, 20, 30 years ago constrain today's choices. The professor has managed university services for nearly 10 years and still deals with architectural decisions from decades ago.

---

## Building Cloud: Not Just Technology

A key lesson: building a cloud infrastructure is not purely a technical problem. There are at least four non-technical dimensions equally critical.

### Governance

Governance is the active distribution of decision-making rights and accountability among the various stakeholders of an organization. Who decides which service to activate? Who approves the budget? Who is responsible in the event of an incident?

The three main models are: **centralized** (a central authority governs all business units), **distributed** (each unit has its own autonomous governance, typical of large organizations), **federated** (a combination of the two, with local autonomy but central coordination).

UniPi operates a predominantly centralized model: the organization's size allows it.

Cloud has introduced new organizational roles:
- **Service manager**: responsible for a single service
- **Account manager**: manages the relationship with the customer/user
- **Cloud architect**: designs the overall cloud architecture
- **Service operation manager**: responsible for the entire infrastructure and all services running on it

### Finance and Procurement

In an Italian public organization, IT spending is governed by specific regulations: direct procurement up to ~€139,000; above that threshold a European tender is required (€214,000+), with a minimum lead time of six months. If a server breaks and a European tender is needed, there is no server for six months.

This means procurement must be **proactive**: future needs must be anticipated, tenders planned before resources are needed. The University of Pisa spends approximately €6 million/year on IT: 80,000 switches, 2,000 access points, datacenter and private cloud.

**Chargeback** is the mechanism by which the cost of cloud resources is attributed to the departments or projects that use them. In a public institution no real money changes hands, but usage is tracked. The Computer Science Department using more VMs than the Philosophy Department should bear a proportionally larger share of costs.

Cloud chargeback models:
- **Pay-as-you-go**: you pay for what you use
- **Subscription**: a fixed fee for a period (e.g., yearly Microsoft 365)
- **By peak**: you pay based on the peak usage reached
- **User-based**: cost per user (the enterprise AI model often works this way: each user gets a token quota)

### Service Level Agreement (SLA)

When your resources are "somewhere in the cloud" and you cannot physically go and check on them, the only available guarantee is **contractual**. An SLA is the part of a contract that specifies service parameters: availability (e.g., 99.9%), maintenance schedules, performance levels, support response times, and compensation in case of violation.

The SLAs for all Microsoft online services, including historical versions in all languages, are public and available on the Microsoft website. Studying one is useful for understanding what a cloud provider actually guarantees.

### Vendor lock-in

Dependency on a single provider is a strategic risk. If the provider discontinues a product, changes license terms, or raises prices, the organization is in trouble. Mitigation strategies: diversifying investments, preferring open APIs and open standards.

![[dcdo_cloud_governance_models.png]]
*Fig. — The three cloud governance models: centralized, distributed, federated.*

---

## Physical Layer and Virtual Layer: Review

The professor spends the final minutes quickly reviewing the physical and virtual layers already covered in the course, confirming that the concepts fit together with what has been seen previously.

The **physical layer** comprises compute (servers, CPUs, GPUs), storage (block, file, object — unified storage as a hybrid configuration), and network (Fibre Channel for storage, iSCSI increasingly less common, converged Ethernet). The original slide notes refer to mechanical disk technologies before SSD became widespread, but the logical categories (block, file, object) remain current.

The **virtual layer** consists of virtual machines (VMDK files, virtual appliances), virtual storage (thin-provisioned LUNs, vSAN), and virtual networks. The **PVLAN** (*Private VLAN*) is worth a note: it is a VLAN that forbids horizontal broadcast between hosts. If you are connected to the university Wi-Fi, you are on a PVLAN — which is why you cannot ping other devices on the same wireless network: the network does not allow direct client-to-client communication, only north-south (client → gateway).

> [!note] Next lectures
>
> The professor announced that the next lecture will start with the **control layer** and **orchestration layer** — the genuinely new parts compared to what has already been covered. Security and operations will follow, then the Friday visit to the San Piero datacenter.

---

> [!question] Possible exam questions
>
> - What are the 5 essential cloud characteristics according to NIST? Explain each with a practical example.
> - What is the difference between IaaS, PaaS, and SaaS? Who manages what in each model?
> - What is meant by public, private, hybrid, and community cloud? What distinguishes a private cloud from a public one?
> - Describe the Cloud Computing Reference Model: what layers does it contain and what are the cross-cutting dimensions?
> - What is rapid elasticity and how does it differ from scalability?
> - What is an SLA and why is it crucial in the cloud context?
> - What does greenfield vs. brownfield mean in the context of cloud deployment?
> - What is chargeback and what pricing models exist?
> - What is vendor lock-in and how can it be mitigated?
