# Reinforcement Learning for Unrelated Parallel Machine Scheduling
- Modeled semiconductor photolithography processes as an unrelated parallel machine scheduling problem with photomask constraints.
- Implemented a policy‑based agent using PyTorch to dynamically dispatch jobs in real‑time as they arrived into the system.
- Applied Policy Optimization with Multiple Optima (POMO) by checking machines in different permutation orders when allocating jobs to increase solution diversity.
- Designed and trained the agent using REINFORCE‑based algorithms with POMO baseline to optimize makespan in dynamic job arrival scenarios.
- Achieved performance metrics that outperformed standard rule‑based algorithms (SPT) and approached constraint programming solution quality.
