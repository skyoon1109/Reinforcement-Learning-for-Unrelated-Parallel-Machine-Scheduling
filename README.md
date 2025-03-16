# 📖Reinforcement Learning for Unrelated Parallel Machine Scheduling
- Modeled semiconductor photolithography processes as an unrelated parallel machine scheduling problem with photomask constraints.
- Implemented a policy‑based agent using PyTorch to dynamically dispatch jobs in real‑time as they arrived into the system.
- Applied Policy Optimization with Multiple Optima (POMO) by checking machines in different permutation orders when allocating jobs to increase solution diversity.
- Designed and trained the agent using REINFORCE‑based algorithms with POMO baseline to optimize makespan in dynamic job arrival scenarios.
- Achieved performance metrics that outperformed standard rule‑based algorithms (SPT) and approached constraint programming solution quality.

# 1️⃣Problem Definition
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

## 2-Step Scheduling
### Step 1: Lot Assignment to Machine Waiting Lists
* Dispatch ready lots to machine waiting lists considering workload and processing time per machine

### Step 2: Lot Processing Order Decision
* Determine the actual processing order of lots in the waiting list
* Immediately assign if a processable lot is in the queue
* Use SPT (Shortest Processing Time) → To minimize makespan

![Image](https://github.com/user-attachments/assets/4aec74a5-9ca5-4890-8792-dedb9293cad8)

# 2️⃣Data Generation Method
* Generate Lot ready times randomly with time intervals
* (Number of Process_IDs) = (Number of Lots) ÷ 2
* (Number of Masks) = (Number of Machines)
* Randomly assign eligible Machines and Masks for each Process_ID
* Set processing times randomly between 20-50

# 3️⃣RL based Scheduling
## Overall Architecture
### Dispatching
* Agent selects a machine and assigns a lot as an action when given a state
* Attempts to implement dynamic scheduling without knowledge of subsequent lot information

### Sequencing
* Select the lot with the shortest processing time (SPT) from processable lots in each machine's waiting list

### Reward
* Return the negative of the final makespan as reward after completing an episode  
![Image](https://github.com/user-attachments/assets/eb250de0-c8e1-4c62-8af3-4411bfa00ce7)

## State Description
- Number of lots in each machine queue
- Availability of each mask (0 or 1)
- Total usage count of each mask
- Processing time of next ready lot on each machine
- Total processing time of each machine
- Completed job count for each machine
- Idle time of each machine
- Total processing time in each machine queue
- Idle status of each machine (1 if idle, 0 if not)

## Environment Step
* Process jobs by cycling through machines in given order
* Increase global time t after completing a full machine cycle
* Take Action if lots are ready
* If no lots are ready, advance simulation to next ready time
* End episode when all lots are processed
![Image](https://github.com/user-attachments/assets/46c445ab-bd87-4990-a8f0-1365110e0d22)

## Agent and REINFORCE Algorithm
* Multi-Layer Perceptron (MLP) policy network structure
* Takes state vector as input when a lot becomes ready and selects one machine as action
* Performs only dispatching role when lots become ready (no subsequent lot information)
* Returns negative makespan as reward after episode completion
* Uses POMO baseline

## Train
* Consider each machine processing order as one POMO Trajectory → For $n$ machines: generate $n!$ trajectories
* Use average of all trajectories as baseline for variance reduction
* Compose each batch with multiple instances, update with batch's average loss
![Image](https://github.com/user-attachments/assets/a0b6574d-6214-4b0a-ba57-bddf27c0cb32)

# 4️⃣Model Comparison with Rule-based & CP
## RL vs Rule-based
* Evaluated using 100 randomly generated instances for each case
* RL shows shorter makespan than simple rule-based model
* Rule-based model assigns lots to machines with lowest accumulated idle time → lacks flexibility

| # of Lots | # of Machine | RL | Rule-based |
|-----------|--------------|-----|------------|
| 10 | 3 | 129.85 | 213.03 |
| 10 | 4 | 118.05 | 175.71 |
| 20 | 3 | 234.31 | 356.15 |
| 20 | 4 | 198.31 | 300.83 |

## RL vs CP
* Evaluated using 10 randomly generated instances for each case
* CP finds optimal solutions while RL shows gap from optimal
* RL executes very quickly while CP execution time increases exponentially with lot count
* CP finds optimal solutions within 10 seconds for 10 lots, but takes up to 5-6 minutes for 20 lots
* Information availability difference:
 * CP knows all lot information in advance to find optimal solutions
 * RL lacks subsequent lot information, only dispatching lots as they become ready

| # of Lots | # of Machine | RL | CP |
|-----------|--------------|-----|-----|
| 10 | 3 | 129.7 | 117.1 |
| 10 | 4 | 120.4 | 99.7 |
| 20 | 3 | 233.5 | 219.1 |
| 20 | 4 | 198.4 | 168.7 |

# 5️⃣Gantt Chart Examples with RL based Schdeduler
- 3 Machines
![Image](https://github.com/user-attachments/assets/2b0e2f7d-ce86-4a02-9989-788590be9c5a)

- 4 Machines
![Image](https://github.com/user-attachments/assets/6d9c1fe0-fe30-46f4-a6e8-a67bc8007409)
