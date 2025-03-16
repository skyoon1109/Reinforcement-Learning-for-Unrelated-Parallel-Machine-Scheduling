# Reinforcement Learning for Unrelated Parallel Machine Scheduling
- Modeled semiconductor photolithography processes as an unrelated parallel machine scheduling problem with photomask constraints.
- Implemented a policy‑based agent using PyTorch to dynamically dispatch jobs in real‑time as they arrived into the system.
- Applied Policy Optimization with Multiple Optima (POMO) by checking machines in different permutation orders when allocating jobs to increase solution diversity.
- Designed and trained the agent using REINFORCE‑based algorithms with POMO baseline to optimize makespan in dynamic job arrival scenarios.
- Achieved performance metrics that outperformed standard rule‑based algorithms (SPT) and approached constraint programming solution quality.

# Problem Definition
## Unrelated Parallel Machine Scheduling with Mask Constraints
- Each lot has a designated Process_ID  
- Each Process_ID has specific eligible Machine sets and Mask sets  
- Processing time varies for each Process_ID on different eligible machines  
- Each Process_ID and Machine pair has different eligible Mask sets  
- A Mask can only be used on one machine at any given time  
- Each lot has a Ready (release) time → Assume lots arrive in real-time  
- Each Machine has a queue → Considering real-world conditions  
- Objective: Minimizing Makespan  

![Image](https://github.com/user-attachments/assets/8892cbc8-10d3-4129-b67e-9a1773b0b1e7)
