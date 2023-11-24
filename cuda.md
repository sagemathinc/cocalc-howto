# Write a C Program Using a GPU and the [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit) on CoCalc.com

## Create a Compute Servers with a T4 GPU

In a https://Cocalc.com, open a project, click on "Servers" in the action bar on the left, then create a compute server that has a GPU. For this tutorial, a T4 GPU spot instance will be fine, and **costs under \$0.20/hour** (you pay by the second). Make sure to select the ["CUDA Toolkit"](https://developer.nvidia.com/cuda-toolkit) image:

<br/>

![](.cuda.md.upload/paste-0.5505944249523818)

<br/>

Click Start to start your compute server running. If it immediately stops that's because no amazingly affordable spot instances in the current region are available, so try editing the region, or switching the server from a Spot instance to a Standard instance (which costs a bit more).  To move to a different region, you have to deprovision the instance first. Availability of spot instances may be much higher in Asia, and using a region there is fine, though typing in a terminal will be a little slower.

## Write a CUDA C program

There are many books and tutorials on CUDA C/C++ programming. Here's a basic "hello world" style program. Click +New and create a new file "add.cu", and copy/paste this code in.

```cu
/*
nvcc add.cu -o add
*/

#include <stdio.h>
#include <stdlib.h>
#define N 10000000

__global__ void vector_add(float *out, float *a, float *b, int n) {
    for(int i = 0; i < n; i++){
        out[i] = a[i] + b[i];
    }
}

int main(){
    float *a, *b, *out;
    float *d_a, *d_b, *d_out;  // device copies of a, b, out

    // Allocate memory on host
    a   = (float*)malloc(sizeof(float) * N);
    b   = (float*)malloc(sizeof(float) * N);
    out = (float*)malloc(sizeof(float) * N);

    // Initialize arrays
    for(int i = 0; i < N; i++){
        a[i] = 1.0f; b[i] = 2.0f;
    }

    // Allocate device copies of a, b, out
    cudaMalloc((void**)&d_a, N * sizeof(float));
    cudaMalloc((void**)&d_b, N * sizeof(float));
    cudaMalloc((void**)&d_out, N * sizeof(float));

    // Copy inputs to device
    cudaMemcpy(d_a, a, N * sizeof(float), cudaMemcpyHostToDevice);
    cudaMemcpy(d_b, b, N * sizeof(float), cudaMemcpyHostToDevice);

    // Launch add() kernel on GPU
    vector_add<<<1,1>>>(d_out, d_a, d_b, N);

    // Copy result back to host
    cudaMemcpy(out, d_out, N * sizeof(float), cudaMemcpyDeviceToHost);

    // Print result on host
    printf("0th component: %f + %f = %f\n", a[0], b[0], out[0]);

    // Cleanup
    free(a); free(b); free(out);
    cudaFree(d_a); cudaFree(d_b); cudaFree(d_out);
}
```

The above program creates a kernel that runs on the GPU that adds two vectors with 10 million entries each together.

## Compile and run your CUDA C program

Open a separate terminal (+New --> Linux Terminal),

<br/>

![](.cuda.md.upload/paste-0.5356796390360037)

<br/>

then in the upper left server selector, select your compute server:

<br/>

![](.cuda.md.upload/paste-0.06586343692390417)

<br/>

Click "sync files" to ensure that the latest version of `add.cu` is on the compute server you just created, then compile your code by typing `nvcc add.cu`

```sh
(compute-server-3) ~/cuda/add$ ls
add.cu  cuda.term
(compute-server-3) ~/cuda/add$ nvcc add.cu

```

Now you can run your code.  Use nvprof to see how much time is spent copying data back and forth to the GPU, and how much time the actual computation on the GPU takes.

```sh
(compute-server-3) ~/cuda/add$ time nvcc add.cu

real    0m4.097s
user    0m0.830s
sys     0m0.365s
(compute-server-3) ~/cuda/add$ time ./a.out
0th component: 1.000000 + 2.000000 = 3.000000

real    0m1.825s
user    0m0.698s
sys     0m0.727s
(compute-server-3) ~/cuda/add$ nvprof ./a.out
==766== NVPROF is profiling process 766, command: ./a.out
0th component: 1.000000 + 2.000000 = 3.000000
==766== Profiling application: ./a.out
==766== Profiling result:
            Type  Time(%)      Time     Calls       Avg       Min       Max  Name
 GPU activities:   91.94%  604.78ms         1  604.78ms  604.78ms  604.78ms  vector_add(float*, float*, float*, int)
                    5.46%  35.919ms         1  35.919ms  35.919ms  35.919ms  [CUDA memcpy DtoH]
                    2.60%  17.086ms         2  8.5432ms  8.4173ms  8.6691ms  [CUDA memcpy HtoD]
      API calls:   61.12%  659.35ms         3  219.78ms  8.6540ms  641.83ms  cudaMemcpy
                   38.29%  413.04ms         3  137.68ms  132.65us  412.75ms  cudaMalloc
                    0.37%  4.0433ms       101  40.033us     145ns  2.2382ms  cuDeviceGetAttribute
                    0.22%  2.3433ms         3  781.09us  264.42us  1.0436ms  cudaFree
                    0.00%  44.939us         1  44.939us  44.939us  44.939us  cudaLaunchKernel
                    0.00%  10.972us         1  10.972us  10.972us  10.972us  cuDeviceGetName
                    0.00%  4.8860us         1  4.8860us  4.8860us  4.8860us  cuDeviceGetPCIBusId
                    0.00%  1.4700us         3     490ns     165ns     933ns  cuDeviceGetCount
                    0.00%  1.0720us         2     536ns     151ns     921ns  cuDeviceGet
                    0.00%     361ns         1     361ns     361ns     361ns  cuDeviceTotalMem
                    0.00%     269ns         1     269ns     269ns     269ns  cuDeviceGetUuid
(compute-server-3) ~/cuda/add$ 
```

## Workflow

Whenever you edit C/C\+\+ cuda code, be sure to click "Sync Files" to copy the latest version to your compute server before compiing and running it.

If you want to use VS Code instead of CoCalc's C editor, click "Servers" scroll down a little, and click "Vs Code...":

<br/>

![](.cuda.md.upload/paste-0.9014775425144061)

<br/>

Then use VS Code in another browser tab to edit code:

<br/>

![](.cuda.md.upload/paste-0.24423574563380268)

## Clean Up

When you are done you can turn off your compute server to only pay for disk storage.  Since you didn't store anything longterm directly on the compute server \(your files are all in your cocalc project\), you can also just deprovision your compute server so there is no cost at all. To do this, click "Edit", then "Deprovision":

![](.cuda.md.upload/paste-0.18832951389387942)

