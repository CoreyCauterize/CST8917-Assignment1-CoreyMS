# CST8917-Assignment1
Name: Corey Mark-Stewart  
Student Number: 040770982  
Course: CST8917  
Title: Assignment 1: Serverless Computing - Critical Analysis  
Date: 06-14-2026  

---

## Part 1: Paper Summary

This paper argues that serverless computing, particularly Functions as a Service (FaaS) platforms such as AWS Lambda, Microsoft Azure Functions, and Google Cloud Functions, limits the future potential of cloud innovation. The authors describe serverless computing as “one step forward, two steps back” (Hellerstein et al., 2019). While FaaS provides advantages such as automatic scaling, a pay-as-you-go pricing model, and reduced infrastructure management, they argue that these benefits come at the cost of restricting efficient data processing and distributed computing (Hellerstein et al., 2019).

When the authors refer to “one step forward,” they are highlighting the advantages of autoscaling infrastructure. Developers can deploy functions without provisioning or managing servers and only pay for the resources they use. This makes development and operations simpler while supporting dynamic workloads efficiently. However, the authors argue that serverless computing sacrifices too many capabilities in exchange for convenience, making it poorly suited for modern cloud applications that depend on large datasets, persistent state, coordination, and specialized computation (Hellerstein et al., 2019).

One major limitation is execution time constraints. In the FaaS model, functions are designed to be short-lived. For example, AWS Lambda imposes a 15-minute execution limit, and other cloud platforms have similar restrictions. These limitations make long-running processes difficult to implement and prevent functions from maintaining state across executions (Hellerstein et al., 2019).

Another issue is communication and networking limitations. In serverless computing, functions cannot be directly addressed or communicate efficiently with one another. Instead, they often rely on intermediary storage services such as object storage systems to exchange data. This approach increases latency and operational costs because functions repeatedly read from and write to storage instead of sharing data directly (Hellerstein et al., 2019).

The authors also identify the data shipping anti-pattern as a major concern. FaaS platforms often require “shipping code to data” instead of enabling computation close to where the data resides. Since functions are isolated from the data source and depend on slower storage systems, overall performance decreases. This architecture also makes coordination between functions more difficult and limits opportunities for innovation (Hellerstein et al., 2019).

Hardware access is another limitation of serverless platforms. Functions are typically allocated limited CPU and memory resources and generally lack access to specialized hardware designed for high-performance tasks such as GPUs. This restriction reduces the effectiveness of workloads such as machine learning, analytics, and scientific computing (Hellerstein et al., 2019).

Additionally, distributed computing and stateful workloads remain difficult in a serverless environment. Since functions are isolated and temporary, maintaining persistent state and coordinating distributed systems becomes challenging. Short execution times, slower communication methods, and synchronization overhead further complicate these workloads (Hellerstein et al., 2019).

To address these issues, the authors propose several improvements for future cloud programming models, including fluid code and data placement, heterogeneous hardware support, and long-running addressable virtual agents (Hellerstein et al., 2019).

---

## Part 2: Azure Durable Functions Deep Dive

### Orchestration Model

Azure Durable Functions expands on the normal Functions as a Service (FaaS) model by adding orchestration. Instead of each function running independently, Durable Functions uses three parts: client functions, orchestrator functions, and activity functions. Client functions are responsible for starting workflows and checking their progress. Orchestrator functions control the workflow and decide what activities run and in what order. Activity functions perform the actual work.

This differs from traditional serverless because regular functions run independently and do not maintain coordination between executions. Durable Functions allows multiple functions to work together as a single workflow and supports long-running processes more effectively. This helps address one of the paper’s criticisms that first-generation serverless platforms make coordination difficult (Microsoft Learn, Durable orchestrations).

---

### State Management

The paper argues that serverless platforms struggle because functions are stateless and temporary. Azure Durable Functions addresses this using event sourcing, checkpointing, and replay.

When an orchestrator function reaches a waiting point, its progress is saved to durable storage. Instead of restarting from the beginning, Durable Functions reloads history and reconstructs state through replay. This allows workflows to survive failures and scaling events without manual state handling.

This directly addresses the paper’s criticism of stateless execution, although it still depends on storage and replay mechanisms internally (Microsoft Learn, Durable Functions code constraints; Durable orchestrations).

---

### Execution Timeouts

One limitation discussed in the paper is short execution time limits. Durable Functions reduces this issue by separating orchestration from execution.

Orchestrators can run long workflows because they persist state between steps rather than staying active. However, activity functions still follow execution limits depending on the hosting plan.

This improves support for long-running applications but does not fully remove execution constraints (Microsoft Learn, Durable orchestrations).

---

### Communication Between Functions

The paper criticizes FaaS because functions cannot directly communicate and often rely on storage systems.

Durable Functions improves this by letting orchestrators coordinate activity functions. Activity results are returned to the orchestrator, which controls execution flow.

However, Durable Functions still relies on storage and messaging systems internally. This reduces communication friction but does not eliminate the underlying dependency on storage systems (Microsoft Learn, Durable orchestrations; Azure Storage provider).

---

### Parallel Execution (Fan-Out/Fan-In)

Durable Functions supports parallel processing using a fan-out/fan-in pattern. The orchestrator starts multiple activity functions in parallel, waits for completion, and then combines results.

This improves distributed computing support and simplifies synchronization and retries for developers.

However, orchestration still depends on storage and scheduling systems, which introduces performance tradeoffs compared to tightly coupled distributed systems (Microsoft Learn, Durable orchestrations).

---

## Part 3: Critical Evaluation

Azure Durable Functions improves several limitations found in first-generation serverless computing, but it does not completely solve the concerns discussed in the paper. Instead, it introduces orchestration and stateful workflows that make serverless more usable while still keeping the same underlying architectural model.

One limitation that is only partially resolved is communication and network overhead. The paper criticizes FaaS platforms for relying on storage systems for coordination. Durable Functions improves coordination through orchestrators, but still depends on storage and messaging systems for persistence and execution tracking. Every step in a workflow requires checkpointing and state reconstruction, which means the underlying communication model is still storage-based even if it is hidden from developers (Microsoft Learn, Azure Storage provider).

Another limitation that remains unresolved is limited hardware access. The paper highlights that serverless platforms do not provide direct access to GPUs or specialized compute resources. Durable Functions does not change the execution environment of Azure Functions, meaning activity functions remain constrained to general-purpose compute resources.

Overall, Azure Durable Functions represents progress toward the authors’ vision but mainly works around limitations rather than removing them. It improves state management, orchestration, and distributed execution, but the core architecture of serverless computing remains unchanged.

## References

Hellerstein, J. M., Faleiro, J. M., Gonzalez, J. E., Schleier-Smith, J., Sreekanti, V., Tumanov, A., & Wu, C. (2019). *Serverless Computing: One Step Forward, Two Steps Back*. Conference on Innovative Data Systems Research (CIDR).  
https://arxiv.org/abs/1812.03651  

Microsoft Learn. (n.d.). *Durable Functions orchestrations overview*.  
https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-orchestrations  

Microsoft Learn. (n.d.). *Durable Functions code constraints*.  
https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-code-constraints  

Microsoft Learn. (n.d.). *Durable Functions bindings*.  
https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-bindings  

Microsoft Learn. (n.d.). *Durable Functions performance and scale*.  
https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-perf-and-scale  

Microsoft Learn. (n.d.). *Azure Storage provider for Durable Functions*.  
https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-azure-storage-provider  
## AI Disclosure Statement

Used to ChatGPt to give an outline, fix spelling and grammar mistakes and suggestions, also to help summarize and explain the reseach paper.


