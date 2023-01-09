# Rodinia Kmeans Optimization Project (R.K.O.P)

This project aims to optimize the Kmeans application from the Rodinia benchmark suite.


### Prerequisites
- GCC
- Cuda toolkit

## How to run
```sh
cd kmeans
make
./run
```

### Using the optimizations
To use one flavour of the optimizations you must modify the Makefile and recompile.
The `version` field in the Makefile sets which version will be compiled. Once you have modified the version variable,
run `make clean && make`.

Version:
- 1: Default, no optimizations
- 2: Streams
- 3: Pinned memory
- 4: Unrolled loop
- 5: Unified Memory
- 99: Best of all versions 


### Advanced usage

./kmeans [switches] -i filename
```
-i filename      :file containing data to be clustered
-m max_nclusters :maximum number of clusters allowed    [default=5]
-n min_nclusters :minimum number of clusters allowed    [default=5]
-t threshold     :threshold value                       [default=0.001]
-l nloops        :iteration for each number of clusters [default=1]
-b               :input file is in binary format
-r               :calculate RMSE                        [default=off]
-o               :output cluster center coordinates     [default=off]
```
## Data generation

You can generate dataset of any size using the data generator inside data/inputGenerator.

### How to run the generator
```sh
cd data/inputGenerator
make
./gen_dataset.sh
```

This will generate datasets of various sizes and format.

#### Advanced usage
./datagen [datasize] 
```
datasize          :Total count of data points
-f data type      :Generates float data instead of int
```

## License
Same license for kmeans applies from the Rodinia benchmark suite at https://www.cs.virginia.edu/rodinia/doku.php?id=start

