# Krittika (Pleiades)
Distibuted ML Accelerator simulator

Plans to support
1. Distributed execution of DNN layers
2. Heterogenous cores
3. Multitenant execution

## *Installing the package*
Getting started is simple! Krittika is completely written in python and uses scalesim-v2 in backend.

You can clone the SCALE-Sim(v3) repository using the following command (ssh)

```$ git clone git@github.com:scalesim-project/scale-sim-v3.git```

Alternative, you can also clone using https 

```$ git clone https://github.com/scalesim-project/scale-sim-v3.git```

If you are running for the first time and do not have all the dependencies installed, please install them first using the following command

```$ pip3 install -r <path_to_scale_sim_repot>/requirements.txt```

After cloning, install scalesim from path using the following command. This version will automatically reflect any code changes that you make.

```$ pip3 install -e <path_to_scale_sim_repo_root>```

## *Launching a run*
Krittika can be run by using the krittika-sim.py script from the repository and providing the paths to the architecture configuration file (refer configs/krittika.cfg for example) and the topology descriptor csv file (refer scalesim repo for examples).

```$ python3 krittika-sim.py -c <path_to_config_file> -t <path_to_topology_file>```

```python3 -m krittika.krittika-sim -c configs/krittika.cfg -t topologies/test.csv -p partitions/temp_part.csv```

Additional optional parameters
1. -p <path_to_partition_file> (Refer partition_manager.py for more info)
2. -o <path_to_the_log_dump_directory> 
3. --verbose <True/False> (Flag to change the verbosity of the run)
4. --savetrace <True/False> (Flag to indicate if the traces should be saved)

## *Topology file*
The topology file is a *CSV* file which decribes the layers of the workload topology. The layers are typically described as convolution/GEMM/activation layer parameters as shown in the example below

![topology file](https://github.com/scalesim-project/krittika/blob/main/documentation/resources/topology%20file.png "topology file")

Conv and GEMM layers follow scalesim topology structure [link](https://scale-sim-project.readthedocs.io/en/latest/topology.html)

The only difference is that there is *no comma* at the end of each layer.

Support for RELU activation is also added and can be used just as shown in the image.


## Experiments for MoE models on TPUs:

Below is a MoE model runs on TPU v4, suppose TPU v4 has 8 compute cores (systolic array),
We set Tensor Parallelism (TP) = 2, Expert Parallelism (EP) = 4,
Run 4 commands below in 4 bash terminals:

```bash
python3 -m krittika.krittika-sim \
-c configs/tpuv4_2core.cfg \
-t topologies/qwen1.5_skew_ep0.csv \
-p partitions/qwen1.5_skew.csv \
-o results/qwen-1.5-dist-tpuv4-ep0

python3 -m krittika.krittika-sim \
-c configs/tpuv4_2core.cfg \
-t topologies/qwen1.5_skew_ep1.csv \
-p partitions/qwen1.5_skew.csv \
-o results/qwen-1.5-dist-tpuv4-ep1

python3 -m krittika.krittika-sim \
-c configs/tpuv4_2core.cfg \
-t topologies/qwen1.5_skew_ep2.csv \
-p partitions/qwen1.5_skew.csv \
-o results/qwen-1.5-dist-tpuv4-ep2

python3 -m krittika.krittika-sim \
-c configs/tpuv4_2core.cfg \
-t topologies/qwen1.5_skew_ep3.csv \
-p partitions/qwen1.5_skew.csv \
-o results/qwen-1.5-dist-tpuv4-ep3
```
