# CSC6109 Assignment-1: EC2 Measurement (2 questions)

### Deadline: 23:59, Sep 23
---

### Name: ZHOU Yixin
### Student Id: 224040090
---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose any open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > I chose sysBench as the measurement tool to test CPU and memory performance. The configurations are as follows:
    > 
    > Region: US East (N. Virginia)
    > 
    > For cpu measurement, the command is `sysbench cpu --cpu-max-prime=20000 run`, where the upper limit for primes generator is set as 20000, twice of the default number and the Number of threads is 1 as default. Compared with the default settings, to generate primes up to 20000 leads to more stress on the operation, which can measure the CPU performance under a more extreme environment. The compute performance differences between each instances can be shown more clearly. Here, I used the `CPU seepd` (events per second) as the metrix, which means how many primes can be calulated within one second in average.
    > 
    > For memory measurement, the command is `sysbench memory --memory-block-size=1M --memory-total-size=10G run`, where the size of memory block for test is 1M and the total size of data to transfer is 10G. As default, the memory access scope is 'global', the type of memory operations is 'write' and the memory access mode is 'seq'. The size of memory block of 1M is to simulate memory visiting mode in some real tasks like database and encoding videos. The total size of 10G can control the total running time within a reasonable horizon. Here, I used the `MiB/sec` as the metrix, which means how much memory is transferred every second.

2. (1 mark) Run your measurement tool on general purpose `t3.medium`, `m5.large`, and `c5d.large` Linux instances, respectively, and find the performance differences among them. Launch all instances in the **US East (N. Virginia)** region. What about the differences among these instances in terms of CPU and memory performance? Please try explaining the differences. 

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size      | CPU performance | Memory performance |
    |-----------|-----------------|--------------------|
    | `t3.medium` | 410.29                |  17944.99                  |
    | `m5.large`  |  410.78               |   17943.82                 |
    | `c5d.large` |   477.67              |   20823.45                 |

    > Region: US East (N. Virginia)
    >
    > Analysis:
    > - Overall, `c5d.large` performs better than `t3.medium` and `m5.large` in terms of both CPU and memory performance. This can be explained by the different AWS EC2 Instance Family Designs.
    > - For `t3`/`m5` family ([general purpose instances](https://docs.aws.amazon.com/ec2/latest/instancetypes/gp.html)), AWS positions these families as “balanced” for a wide range of workloads (web servers, small databases, etc.). They prioritize overall balance between CPU, memory, and networking—not specialized performance in one area. Thus, their CPU and memory capabilities are tuned to be adequate for general tasks but not optimized for extreme compute or memory throughput.
    > - For `c5d` family ([compute optimized instances](https://docs.aws.amazon.com/ec2/latest/instancetypes/co.html)), AWS designs `c5`/`c5d` instances for compute-intensive workloads (e.g., batch processing, high-performance computing). `c5d` instances are featured by faster CPU architectures (e.g., higher clock speeds, more efficient instruction sets like AVX-512 for arithmetic tasks), optimized memory subsystems (faster memory controllers, higher bandwidth between CPU and RAM) to support compute-heavy jobs that rely on fast data access. Also, the `c5d` variant includes local NVMe storage, which reduces bottlenecks in I/O-heavy tasks and can indirectly improve memory performance by offloading some data handling to ultra-fast local storage.
## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.  

    | Type          | TCP b/w (Gbps) | RTT (ms) |
    |---------------|----------------|----------|
    | `t3.medium`-`t3.medium` |   4080             |  0.322        |
    | `m5.large`-`m5.large`  |    4930            |   0.206       |
    | `c5n.large`-`c5n.large` |   4940             |   0.314       |
    | `t3.medium`-`c5n.large`   | 3800               |  0.384        |
    | `m5.large`-`c5n.large`  |   2340             |   0.732       |
    | `m5.large`-`t3.medium` |  2020              |  0.892        |

    > Region: US East (N. Virginia)
    >
    > Between the same type of instances, the TCP b/w is high (4080-4930 Mbps) and the RTT is short (0.206-0.322 ms). This result shows that network performance between same-type instances can utilize the resources more efficiently and the latancy is low.
    >
    > Between different types of instances, the TCP b/w is low (2020-3800 Mbps) and the RTT is long (0.384-0.892 ms). This result shows that network utility between different types is inefficent and the responce speed of network is slow.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection | TCP b/w (Mbps)  | RTT (ms) |
    |------------|-----------------|--------------------|
    | N. Virginia-Oregon |    480             |    60.371               |
    | N. Virginia-N. Virginia  |   4940              |   0.021                 |
    | Oregon-Oregon |   4970              |  0.129                  |

    > All instances are `c5.large`.
    > The results show that network between different regions is much worse than that between the same regions.

