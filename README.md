## Table of Contents
- [About](#About)
- [Usage](#Usage)
- [Result](#Result)
- [Visualizer](#Visualizer)
- [Circuits](#Circuits)
- [Contribution](#Contribution)

## About
In this project, we implement an approach that reduces the depth of quantum circuits, specifically focusing on the CNOT gate.
```bash
.
├── Matrices/
├── Config/
├── Results/
├── Circuits/
├── main.py
    ├── DepthOrientedGreedy.py
        ├── RowGreedy.py
        ├── ColGreedy.py
        ├── Row_or_Col_Greedy.py
        ├── ParallelGreedy.py
            ├── cost_function.py
            ├── operations.py
            ├── selector.py
├── in-place_klein.py
├── out-of-place_klein.py
└── randMatGenerator.py
```
## Usage
### Specify the greedy algorithm and cost function type for a single execution
```bash
python3 main.py <Matrix> <Greedy Selection> <Cost Mat Function> <Times>
```
- Matrix: The matrix what we testing.
- Greedy Selection: The greedy algorithm you want to execute. We provide four types of algorithms: Row, Column, Row_or_Col, and Parallel.
- Cost Mat Function: The cost mat function used to calculate the operation cost. You can choose from sum, square, origin and log. The function options mapping to 1, 2, 3 and -1.
- Times: The number of iterations you want to run.

For example, if you would like to execute a row_or_Col Greedy algorithm with origin cost function,
```bash
python3 main.py Matrix/AES.txt Row_or_Col 3 1
```
### Execute all greedy algorithms with cost function combinations using multi-threading
```bash
python3 main.py <Matrix> all <Times>
```
- Matrix: The matrix what we testing.
- Times: The number of iterations you want to run.
The others will be taken from a combination list as input.

For example, if you would like to execute it 1,000 times on the AES.txt matrix,
```bash
python3 main.py Matrix/AES.txt all 1000
```
## Result
The algorithm generates the CNOT synthesis result record in files, for example, we will make two results
### Layer Results
This result presents the available operators for each layer and records the number of CNOT gates and the circuit depth of the quantum circuit.

Each operator is denoted as (i, j, operation), where 0 represents a row operation and 1 represents a column operation.
```bash
(30 14 1)|(31 23 1)|(15 7 1)|(22 6 1)|(4 20 1)|(29 13 1)|(28 12 1)|(2 18 1)|(21 5 1)|(27 19 1)|(1 25 1)|(9 17 1)|(3 11 1)|(8 24 1)|(10 26 1)|(16 0 1)|
(23 7 1)|(13 5 1)|(29 21 1)|(9 1 1)|(11 19 1)|(3 27 1)|(15 31 1)|(17 24 1)|(30 22 1)|(0 16 1)|(28 4 1)|(14 6 1)|(20 12 1)|(2 10 1)|(25 8 1)|(18 26 1)|
(19 18 1)|(5 20 1)|(25 17 1)|(6 13 1)|(11 15 1)|(7 14 1)|(0 31 1)|(26 1 1)|(24 8 1)|(27 23 1)|(12 28 1)|
(1 23 1)|(25 24 1)|(12 31 1)|(18 16 1)|(26 2 1)|(13 4 1)|(5 29 1)|(19 3 1)|(8 15 1)|(14 21 1)|(6 30 1)|(17 9 1)|(20 11 1)|(27 10 1)|
(11 10 1)|(7 15 1)|(24 23 1)|(19 31 1)|(5 4 1)|
(24 22 1)|
(15 24 0)|(23 25 0)|(10 20 0)|
(22 31 0)|(15 4 0)|(23 11 0)|(16 25 0)|(1 18 0)|(10 12 0)|(27 20 0)|
(30 23 0)|(29 22 0)|(9 10 0)|(8 1 0)|(3 4 0)|(2 27 0)|(28 21 0)|(11 12 0)|(31 24 0)|(15 16 0)|(0 17 0)|(25 18 0)|
(13 29 0)|(7 23 0)|(1 9 0)|(27 3 0)|(31 15 0)|(19 11 0)|(5 21 0)|(22 30 0)|(4 28 0)|(6 14 0)|(8 0 0)|(17 25 0)|(10 2 0)|(12 20 0)|(26 18 0)|(16 24 0)|
(5 13 0)|(21 29 0)|(14 30 0)|(6 22 0)|(24 8 0)|(19 27 0)|(12 28 0)|(7 15 0)|(18 2 0)|(11 3 0)|(17 9 0)|(25 1 0)|(23 31 0)|(20 4 0)|(26 10 0)|(0 16 0)|
CNOT: 117, depth: 11 and cost function: origin occurs in 0
Input-wire Permutation: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31]
```
### Sequence Results
Another result presents the synthesized operations of this quantum circuit. This makes it convenient to verify correctness, as we can execute each operator on a matrix to check whether it produces a permutation matrix.
```bash
7, 15, 1
6, 22, 1
14, 30, 1
23, 31, 1
28, 12, 1
13, 29, 1
20, 4, 1
21, 5, 1
27, 3, 1
26, 10, 1
2, 18, 1
24, 8, 1
11, 19, 1
25, 1, 1
9, 17, 1
16, 0, 1
31, 15, 1
5, 29, 1
27, 11, 1
21, 13, 1
19, 3, 1
17, 1, 1
0, 7, 1
25, 9, 1
18, 2, 1
22, 14, 1
20, 28, 1
12, 23, 1
16, 24, 1
7, 23, 1
10, 9, 1
15, 22, 1
1, 8, 1
29, 28, 1
30, 5, 1
26, 25, 1
3, 27, 1
11, 31, 1
0, 16, 1
18, 17, 1
4, 20, 1
6, 14, 1
4, 23, 1
22, 5, 1
18, 9, 1
12, 11, 1
19, 7, 1
15, 30, 1
8, 31, 1
3, 10, 1
1, 25, 1
29, 21, 1
4, 11, 1
5, 28, 1
17, 7, 1
22, 14, 1
30, 6, 1
1, 16, 1
3, 23, 1
9, 31, 1
29, 4, 1
9, 7, 1
22, 13, 1
8, 23, 1
6, 22, 0
7, 28, 0
26, 11, 0
19, 4, 0
14, 23, 0
20, 12, 0
25, 2, 0
16, 8, 0
26, 19, 0
21, 22, 0
7, 24, 0
28, 15, 0
31, 4, 0
17, 3, 0
6, 23, 0
5, 15, 0
12, 29, 0
0, 1, 0
18, 3, 0
24, 9, 0
25, 11, 0
20, 13, 0
2, 19, 0
26, 10, 0
16, 17, 0
6, 31, 0
21, 14, 0
27, 28, 0
26, 18, 0
14, 6, 0
2, 10, 0
24, 16, 0
28, 20, 0
3, 19, 0
9, 25, 0
29, 13, 0
1, 17, 0
23, 7, 0
11, 27, 0
15, 31, 0
5, 21, 0
0, 16, 0
18, 2, 0
10, 26, 0
12, 28, 0
31, 23, 0
19, 11, 0
1, 25, 0
17, 9, 0
15, 7, 0
4, 20, 0
3, 27, 0
8, 24, 0
22, 6, 0
30, 14, 0
13, 21, 0
29, 5, 0
CNOT: 121
```
## Visualizer
### Single Visualizer
The single visualizer can plot the one results.
```bash
python3 visualizer_single.py Row_AES-32-block_sq_Layer_Results 
```
### All Visualizer
The all visualizer can plot four types of results.
```bash
python3 visualizer_all.py Row_AES-32-block_sq_Layer_Results Col_AES-32-block_sq_Layer_Results Row_or_Col_AES-32-block_sq_Layer_Results Parallel_AES-32-block_sq_Layer_Results
```

## Circuits
In this project, we use [ProjectQ](https://github.com/ProjectQ-Framework/ProjectQ) to draw our circuit figures. After you obtain the `.tex` file via our script and replace it with your synthesized gates, you can compile it using the `pdflatex` compiler with the following command:

```bash
pdflatex yourcircuit.tex
```

## Contribution
This project was completed with the support of [ACADEMIA SINICA](https://www.sinica.edu.tw/en). The main supporter was [Dr. Tung Chou](https://tungchou.github.io/), who proposed the two core algorithms, Row-or-Col and Parallel, which achieve lower-depth circuits in the search for block cipher implementations.
