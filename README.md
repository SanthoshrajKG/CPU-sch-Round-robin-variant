# CPU Scheduling with Dynamic Time Allocation and Burst Time Prediction Using Machine Learning

## Overview

CPU scheduling is a fundamental function of an operating system that determines the order in which processes are executed by the CPU. Efficient scheduling aims to minimize waiting time, turnaround time, and scheduling overhead while improving overall system performance.

This project implements and compares four CPU scheduling algorithms: **First Come First Serve (FCFS), Shortest Job First (SJF), Round Robin (RR), and a Modified Round Robin algorithm with dynamic time allocation**.

The project also incorporates a **K-Nearest Neighbors (KNN) machine learning model** to predict process burst times using Python. The predicted burst times can help inform scheduling decisions and support more efficient allocation of CPU execution time.

## Objectives

- Implement FCFS, SJF, and Round Robin scheduling algorithms using C.
- Design a modified Round Robin algorithm with dynamically calculated time quantum.
- Reduce context-switching overhead and improve scheduling performance.
- Compare scheduling algorithms using average waiting time and average turnaround time.
- Develop a KNN-based machine learning model to predict process burst times.
- Evaluate scheduling performance for increasing, decreasing, and randomly ordered process burst times.

## Features

- **FCFS Scheduling:** Executes processes in their arrival order.
- **Shortest Job First:** Prioritizes processes with shorter burst times.
- **Round Robin:** Allocates CPU time using a fixed time quantum of 10 time units.
- **Modified Round Robin:** Calculates a time quantum from the statistical properties of process burst times.
- **KNN Burst Time Prediction:** Uses machine learning to estimate process burst times in Python.
- **Performance Evaluation:** Calculates waiting time, turnaround time, and average scheduling metrics.
- **Algorithm Comparison:** Evaluates traditional and modified scheduling approaches under different process orderings.

## Technologies Used

- **C:** Implementation of CPU scheduling algorithms.
- **Python:** Implementation of the machine learning component.
- **K-Nearest Neighbors (KNN):** Burst-time prediction.
- **Matplotlib:** Visualization and comparison of scheduling performance.
- **Statistical Analysis:** Mean, median, and standard deviation for dynamic time-quantum calculation.

## Scheduling Algorithms

### 1. First Come First Serve (FCFS)

FCFS executes processes in the order in which they are provided. Each process runs until completion before the next process starts.

**Advantages**
- Simple to implement.
- Low scheduling overhead.

**Limitations**
- Long processes can delay shorter processes.
- Average waiting time can be high for unfavorable process orderings.

### 2. Shortest Job First (SJF)

SJF sorts processes by burst time and executes the shortest process first.

**Advantages**
- Can reduce average waiting time when burst times are known.
- Favors short processes.

**Limitations**
- Requires knowledge or estimation of burst times.
- Long processes may experience starvation in some workloads.

### 3. Round Robin (RR)

Round Robin assigns each process a fixed CPU time quantum. If a process does not finish within its allocated quantum, it is placed back into the scheduling cycle with its remaining execution time.

In this implementation, the time quantum is fixed at **10 time units**.

**Advantages**
- Provides repeated CPU access to runnable processes.
- Suitable for time-sharing environments.

**Limitations**
- A small time quantum can cause frequent context switches.
- A large time quantum can reduce responsiveness and make scheduling resemble FCFS.

### 4. Modified Round Robin with Dynamic Time Allocation

The modified algorithm sorts processes by burst time and calculates a time quantum based on the burst-time distribution.

The implementation uses the following statistical quantities:

- Mean burst time
- Median burst time
- Standard deviation of burst times

The dynamic time quantum is calculated as:

\[
Q = \operatorname{int}\left(\text{Median} - 0.5\sigma\right)
\]

where \(\sigma\) represents the population standard deviation of the burst times.

The algorithm processes each process using the calculated quantum. Processes that have remaining execution time are placed into a new queue for subsequent rounds.

**Intended benefits**
- Adapt the time quantum to the characteristics of the workload.
- Reduce unnecessary scheduling cycles for suitable workloads.
- Improve waiting-time and turnaround-time performance under certain process distributions.

The effectiveness of the modified algorithm depends on the workload and should be evaluated experimentally against traditional Round Robin.

## Machine Learning: Burst Time Prediction

The project includes a K-Nearest Neighbors (KNN) model implemented in Python to predict CPU burst times.

KNN estimates a process's burst time by comparing its input features with similar observations in the training dataset. The burst-time prediction component is intended to provide estimates that can inform CPU scheduling decisions.

### Workflow

1. Collect process-related features and historical burst-time observations.
2. Prepare and preprocess the dataset.
3. Split the data into training and testing sets.
4. Train the KNN regression model.
5. Predict burst times for unseen processes.
6. Evaluate prediction performance using appropriate regression metrics.
7. Use predicted burst times as inputs to scheduling experiments.

**Important:** The KNN model predicts burst time; it does not directly guarantee better scheduling performance. Its effectiveness must be assessed by evaluating both prediction accuracy and the resulting scheduling metrics.

## Performance Evaluation

The project compares traditional Round Robin with the modified algorithm for three burst-time orderings:

- Increasing order
- Decreasing order
- Random order

### Average Waiting Time

| Process ordering | Modified Round Robin | Round Robin |
|---|---:|---:|
| Increasing | 101.70 | 89.50 |
| Decreasing | 113.80 | 172.25 |
| Random | 197.50 | 208.45 |

### Average Turnaround Time

| Process ordering | Modified Round Robin | Round Robin |
|---|---:|---:|
| Increasing | 112.20 | 99.50 |
| Decreasing | 125.30 | 183.75 |
| Random | 219.50 | 230.00 |

*Note: These values are the results reported in the project review. They should be interpreted in the context of the experimental workloads and scheduling implementation.*

### Results Discussion

The reported results show that the modified algorithm performs differently depending on the process ordering.

- **Increasing order:** Traditional Round Robin achieves lower average waiting and turnaround times.
- **Decreasing order:** The modified algorithm achieves lower average waiting and turnaround times.
- **Random order:** The modified algorithm achieves modest improvements in both reported metrics.

These results suggest that dynamic time allocation may benefit certain workloads, but it does not outperform traditional Round Robin in every scenario.

The reported metrics are in time units determined by the experiment; they should not be interpreted as seconds unless the simulation defines them that way.

## Performance Metrics

### Waiting Time

The total time a process spends waiting in the ready queue.

For a process that arrives at time zero:

\[
WT = TAT - BT
\]

### Turnaround Time

The total time from process arrival to completion.

\[
TAT = CT - AT
\]

where:
- \(CT\) = completion time
- \(AT\) = arrival time
- \(BT\) = burst time

### Average Waiting Time

\[
AWT = \frac{\sum_{i=1}^{n} WT_i}{n}
\]

### Average Turnaround Time

\[
ATT = \frac{\sum_{i=1}^{n} TAT_i}{n}
\]

### Throughput

The number of processes completed per unit of elapsed simulation time.

\[
\text{Throughput} = \frac{n}{\text{Total elapsed time}}
\]

### Context Switches

The number of times CPU execution changes from one process to another. Context switches can be counted during simulation to compare scheduling overhead more directly.

## Project Structure

A suggested repository structure is:

```text
cpu-scheduling-ml/
├── c/
│   └── cpu_scheduling.c
├── python/
│   ├── burst_time_prediction.py
│   └── visualize_results.py
├── data/
│   └── burst_time_dataset.csv
├── results/
│   ├── waiting_time_comparison.png
│   └── turnaround_time_comparison.png
├── README.md
└── requirements.txt
```

The filenames above are suggested conventions; adjust them to match the actual files in your repository.

## Installation and Execution

### Prerequisites

- GCC or another compatible C compiler
- Python 3
- pip

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd cpu-scheduling-ml
```

### 2. Compile the C implementation

```bash
gcc c/cpu_scheduling.c -o cpu_scheduling -lm
```

The `-lm` flag links the mathematical library required by the `sqrt()` function.

### 3. Run the scheduling simulation

```bash
./cpu_scheduling
```

Enter the number of processes and their burst times when prompted. The program executes the implemented scheduling algorithms and displays their scheduling order, waiting times, turnaround times, and average metrics.

### 4. Set up Python

```bash
python -m venv .venv
```

Activate the environment:

**Linux/macOS**
```bash
source .venv/bin/activate
```

**Windows**
```powershell
.venv\Scripts\activate
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

Typical dependencies for the machine learning and visualization components include:

```text
numpy
pandas
scikit-learn
matplotlib
```

### 5. Run burst-time prediction

```bash
python python/burst_time_prediction.py
```

### 6. Generate comparison graphs

```bash
python python/visualize_results.py
```

The Python commands assume the files follow the suggested structure and expose the corresponding scripts.

## Limitations and Future Improvements

- Integrate the KNN predictions directly into the C scheduling simulator or implement the complete scheduling pipeline in Python.
- Evaluate prediction accuracy using MAE, RMSE, and \(R^2\).
- Count context switches explicitly instead of inferring overhead from waiting time.
- Test a wider range of workloads, process counts, and burst-time distributions.
- Handle process arrival times rather than assuming all processes arrive at time zero.
- Validate the dynamic quantum for small burst times to prevent a zero or negative time quantum.
- Improve memory management and input validation in the C implementation.
- Compare the proposed method against additional scheduling algorithms and statistical baselines.
- Evaluate fairness, response time, throughput, and scheduling overhead alongside average waiting and turnaround times.

## Conclusion

This project explores CPU scheduling through classical algorithms, a modified Round Robin approach with dynamically calculated time quantum, and machine learning-based burst-time prediction.

By combining C-based scheduling simulation with Python-based KNN regression and performance visualization, the project provides a framework for investigating how workload characteristics and burst-time estimates influence CPU scheduling.

The experimental results indicate that the modified approach can improve scheduling metrics for some process orderings, although it does not consistently outperform traditional Round Robin. Further evaluation across diverse workloads is needed to establish its overall effectiveness.

## Authors

- Enrolment ID: 19BCE1713
- Enrolment ID: 19BME1305
