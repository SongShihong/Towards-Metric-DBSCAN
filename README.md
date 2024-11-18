# Towards Metric DBSCAN: Exact, Approximate, and Streaming Algorithms

This repository is the official implementation of **Towards Metric DBSCAN: Exact, Approximate, and
Streaming Algorithms**.

The Faster Exact Metric DBSCAN Algorithm( Algorithm 1 ) , The Metric $\rho$-approximate DBSCAN Via Core Points Summary Algorithm( Algorithm 2 ) and The Alternative Metric $\rho$-approximate DBSCAN Algorithm( Algorithm 3 ) are implemented by c++.

The Streaming $\rho$-approximate DBSCAN Algorithm( Our streaming algorithm ) is implemented by python.

## Requirements
***

To install requirements of our Our streaming algorithm:

```{Shell}
pip install -r requirements.txt
```
## Main Organization of the Code

***

* **datasets:** this folder contains the clustering datasets we used in the paper.

* **linux:** this folder contains executable files for the Linux environment. For example, Alg1_exec is the executable file of Algorithm 1, Alg1_metric_exec is the executable file of Algorithm 1 with Manhattan distance.

* **result:** this folder is used to store the output of the algorithms.
* **src:** this folder contains the C++ code of our algorithms.

## Compile and Run

***

### Algorithm 1, 2 and 3

**run** algorithm 1, 2 and 3 in the following way:

```{Shell}
./linux/Alg1_exec {dimension} {n} {MinPts} {epsilon} {rho} {dataset file path} {result output path} {centers output path}
```

* **{dimension}:** the dimension of dataset
* **{n}:** number of data items
* **{MinPts}:** DBSCAN parameter $MinPts$
* **{epsilon}:** DBSCAN parameter $\epsilon$
* **{rho}:** approximate DBSCAN parameter $\rho$, note that we set $\rho = 0.5$ when execute our exact algorithms. According to our paper, $\rho \le 2$ for Algorithm 2 and $\rho \le 0.5$ for Algorithm 3.
* **{dataset file path}:** path to dataset
* **{result output path}:** path to the output file of clustering results
* **{centers output path}:** path to the output file of radius-guided k-center results
  
For example,  

```{Shell}
./linux/Alg1_exec 2 4811 4 0.03 0.5 ./datasets/banana ./result/output_result.txt ./result/output_center.txt
```

**compile**

If necessary, we can recompile the executable files in the following way:

```{Shell}
g++ -O3 -std=c++11 -o ./linux/Alg1_exec ./src/Algorithm1/*.cpp
```

and so do the other algorithms.

### Our streaming algorithm

Our streaming algorithm is implemented by python.

Before running the program, please ensure that the following libraries are installed in your Python environment.

* PyTorch 2 (cuda is not necessary)
* numpy
* tqdm

To run the example program, please follow these steps:
1. Change the current directory to `src/StreamingAlg`.
2. Enter the command `make moons` to run the program with the `Moons` dataset.
3. The `ARI` and `AMI` indices will be displayed on the screen.
4. If your environment does not support the `make` command, you can open the `Makefile` file with a text editor and manually enter the commands from there into the command line, for example, if you want to run Our streaming algorithm on `Moons` dataset, you need to enter:
    ```{shell}
    moons:
	python ./DBSCAN.py \
		--data=../../datasets/moons_data.csv \
		--buffer_size=100 \
		--rho=0.5 \
		--eps=2 \
		--minpts=10 \
		--label_out=./output/moons_label.csv
	python ./Analysis.py \
		--y=../../datasets/moons_label.csv \
		--label=./output/moons_label.csv
    ```


This folder contains five files: 
1. `main.py`: our main code for streaming approximate DBSCAN.
1. `DBSCAN.py`: Another version for streaming approximate DBSCAN.
2. `Analysis.py`: calculate ARI and AMI.
3. `PlotCenter.py`: plot the center of balls.
4. `PlotCore.py`: plot the core points.
5. `PlotLabel.py`: plot the label of points.

## Results

***

### Algorithm 1, 2 or 3

When the user runs Algorithm 1, 2 or 3, the program will output the following format to the output file specified by the user: 

Each row represents the coordinates of a data point, with the last column indicating the clustering result (cluster ID) of that data point.

In addition, the program will output some statistical results to the screen.

For example, if the user enters the following command:

```{Shell}
./linux/Alg1_exec 2 4811 4 0.03 0.5 ./datasets/banana ./result/output_result.txt ./result/output_center.txt
```
then the content in  ./result/output_result.txt will become:

![file_result](./src/Algorithm1/plot/file%20result.png)

and the following content will be output to the screen:

![screen_result](./src/Algorithm1/plot/screen%20result.png)


### Our streaming algorithm

First, we need to change the current directory to `src/Algorithm4`.

Then we can run Our streaming algorithm by entering the following command:
```
make moons
```
Then the following result will be displayed in screen.

```
data size =  (9948, 2)
Data load completed.
The first pass: 100%|██████████████████████████████| 78/78 [00:01<00:00, 68.75it/s]
The second pass: 100%|████████████████████████████| 78/78 [00:00<00:00, 110.55it/s]
The third pass: 100%|█████████████████████████████| 78/78 [00:00<00:00, 833.19it/s]
Merge inside S_*: 100%|████████████████████████| 220/220 [00:00<00:00, 7112.44it/s]
The fourth pass: 100%|████████████████████████████| 78/78 [00:00<00:00, 419.08it/s]
|E|= 407
|M| =  1193
|S_*| =  220
Number of clusters  3
the total memory usage / n =  0.16083634901487737
clustering completed with 2.1617186069488525 s
ARI =  0.9675238354642742
AMI =  0.9337253148748574
```
Besides, the program will store the label of all points on src/Algorithm4/output/moons_label.csv

More over, the program will plot the data with label (only Moons and Cluto) and save the image in src/Algorithm4/plot/moons_label.png

![moons_label](./src/Algorithm4/plot/moons_label.png)

If you want to change data set, please enter the corresponding command in this table.

|   Dataset  |  ARI |  AMI |      Command      |
|:----------:|:----:|:----:|:-----------------:|
|    Moons   | 0.94 | 0.90 |     make moons    |
|    Cluto   | 0.94 | 0.92 |     make cluto    |
|    Iris    | 0.57 | 0.73 |     make iris     |
| Arrhythmia | 0.22 | 0.15 |  make arrhythmia  |
|   Cancer   | 0.70 | 0.52 | make breastcancer |
|   Digits   | 0.47 | 0.71 |    make digits    |
|    Ecoli   | 0.57 | 0.53 |     make ecoli    |