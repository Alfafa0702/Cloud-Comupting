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

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.  

    | Type          | TCP b/w (Mbps) | RTT (ms) |
    |---------------|----------------|----------|
    | `t3.medium`-`t3.medium` |                |          |
    | `m5.large`-`m5.large`  |                |          |
    | `c5n.large`-`c5n.large` |                |          |
    | `t3.medium`-`c5n.large`   |                |          |
    | `m5.large`-`c5n.large`  |                |          |
    | `m5.large`-`t3.medium` |                |          |

    > Region: US East (N. Virginia)

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection | TCP b/w (Mbps)  | RTT (ms) |
    |------------|-----------------|--------------------|
    | N. Virginia-Oregon |                 |                    |
    | N. Virginia-N. Virginia  |                 |                    |
    | Oregon-Oregon |                 |                    |

    > All instances are `c5.large`.

