include ../../common/make.config

# C compiler
CC = gcc
CC_FLAGS = -g -fopenmp -O2 

VERSION = 5
# CUDA compiler
NVCC = $(CUDA_DIR)/bin/nvcc
NVCC_FLAGS = -I$(CUDA_DIR)/include --default-stream per-thread

# 'make dbg=1' enables NVCC debugging
ifeq ($(dbg),1)
	NVCC_FLAGS += -g -O0
else
	NVCC_FLAGS += -O2
endif

# 'make emu=1' compiles the CUDA kernels for emulation
ifeq ($(emu),1)
	NVCC_FLAGS += -deviceemu
endif


kmeans: cluster.o getopt.o kmeans.o kmeans_clustering.o kmeans_cuda.o rmse.o
	$(CC) $(CC_FLAGS) cluster.o getopt.o kmeans.o kmeans_clustering.o kmeans_cuda.o rmse.o -o kmeans -L$(CUDA_LIB_DIR) -lcuda -lcudart -lm 

%.o: %.[ch]
	$(CC) $(CC_FLAGS) $< -c

ifeq ($(VERSION),1)
kmeans_cuda.o: kmeans_cuda.cu 
	$(NVCC) $(NVCC_FLAGS) -c kmeans_cuda.cu
	$(info using verision 1: no optimization)
endif

# different improvments
ifeq ($(VERSION),2)
kmeans_cuda.o: kmeans_cuda_v2.cu 
	$(NVCC) $(NVCC_FLAGS) -c kmeans_cuda_v2.cu -o kmeans_cuda.o
	$(info using verision 2: streams)
endif

ifeq ($(VERSION),3)
kmeans_cuda.o: kmeans_cuda_v4.cu 
	$(NVCC) $(NVCC_FLAGS) -c kmeans_cuda_v4.cu -o kmeans_cuda.o
	$(info using verision 3: memory transfers )
endif

ifeq ($(VERSION),4)
kmeans_cuda.o: kmeans_cuda_v4.cu 
	$(NVCC) $(NVCC_FLAGS) -c kmeans_cuda_v4.cu -o kmeans_cuda.o
	$(info using verision 4: invert mapping )
endif

ifeq ($(VERSION),5)
kmeans_cuda.o: kmeans_cuda_v5.cu
	$(NVCC) $(NVCC_FLAGS) -c kmeans_cuda_v5.cu -o kmeans_cuda.o
	$(info using verision 5: unified memory)
endif

ifeq ($(VERSION),99)
kmeans_cuda.o: kmeans_cuda_v5.cu
	$(NVCC) $(NVCC_FLAGS) -c kmeans_cuda_v5.cu -o kmeans_cuda.o
	$(info using verision 99: all)
endif

clean:
	rm -f *.o *~ kmeans kmeans_cuda.linkinfo
