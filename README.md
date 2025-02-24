[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name:  Jiawen Zheng
### Student Id:  21110250
### Email:  jzhengbx@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    - Name of measurement tool: **Phoronix Test Suite**

    - Configuration: 

      - CPU Test: `phoronix-test-suite run pts/compress-7zip`

        - `phoronix-test-suite`:  This is the main command for the **Phoronix Test Suite**, a cross-platform, open-source benchmarking tool, which is used to execute performance tests on hardware and software.
        - `run`: This is a subcommand of the Phoronix Test Suite, used to **execute a specific test suite or test case**. When using `run`, the Phoronix Test Suite will perform the specified test and generate results.
        - `pts/compress-7zip`:  This is the name of the test case to be executed. `pts` stands for **Phoronix Test Suite**, indicating that this is a standard test provided by the suite. `compress-7zip` is the specific test name, which focuses on **7-Zip compression and decompression performance**.

      - Memory Test: `phoronix-test-suite run pts/ramspeed`

        - RAMspeed is designed to test the **memory bandwidth and latency** of the system by performing various memory operations.

        - ```yaml
          Memory Test Configuration
              1: Copy
              2: Scale
              3: Add
              4: Triad
              5: Average
              6: Test All Options
              ** Multiple items can be selected, delimit by a comma. **
              Type: 1
          
          
              1: Integer
              2: Floating Point
              3: Test All Options
              ** Multiple items can be selected, delimit by a comma. **
              Benchmark: 1
          ```

        - Selected **`1` (Copy)**, which means the test will only measure the **copy operation**.

        - Selected **`1` (Integer)**, which means the test will only measure memory performance for **integer operations**.

    - Result Analysis(Take results from `t2.micro` as the example)
      - CPU Test
        - **Compression Rating (3809 MIPS)**: The EC2 instance can compress data at a rate of 3.809 billion instructions per second.
        - **Decompression Rating (3118 MIPS)**: Your EC2 instance can decompress data at a rate of 3.118 billion instructions per second.
        - **Consistency**: The results are very consistent, with deviations for compression test and decopression test as **0.57%** and **0.22%**, respectively.
      - Memory Test
        - **Memory Bandwidth**: The EC2 instance achieved an average memory bandwidth of **11040.29 MB/s** for integer copy operations.
        - **Consistency**: The results are very consistent, with a deviation of only **0.23%**.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | Compression Rating: **3809 MIPS**(avg); Decompression Rating: **3118 MIPS**(avg) | **11040.29 MB/s**(avg) |
    | `t2.medium`  | Compression Rating: **9933 MIPS**(avg); Decompression Rating: **5858 MIPS**(avg) | **19485.77 MB/s**(avg) |
    | `c5d.large` | Compression Rating: **7741 MIPS**(avg); Decompression Rating: **5165 MIPS**(avg) | **13598.95 MB/s**(avg) |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |                |          |
    | `m5.large` - `m5.large`   |                |          |
    | `c5n.large` - `c5n.large` |                |          |
    | `t3.medium` - `c5n.large` |                |          |
    | `m5.large` - `c5n.large`  |                |          |
    | `m5.large` - `t3.medium`  |                |          |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |                |          |
    | N. Virginia - N. Virginia |                |          |
    | Oregon - Oregon           |                |          |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
