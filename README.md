[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/2CuuzvUr)
# assignment-7
module load nvhpc
module load craype-accel-nvidia80

Exercise 1: 
nvc++ laplace2d.cpp -mp -Ofast -o laplace_cpu   
srun -p gpu --cpus-per-task=4 --mem-per-cpu=2000     --time=00:05:00 ./laplace_cpu  
Result: 1873.58 ms  

Original method to compine and run in README.md: 
nvc++ -mp=gpu -gpu=cc80 -Ofast laplace2d.cpp -o laplace -Minfo=accel,mp
srun -p gpu --gres=gpu:1 --ntasks=1 --time=00:05:00 --mem=40G ./laplace
(and --reservation=p_es_itkpp_204 if lab time)
Result outside lab time: 1665.96 ms  

------  
nvc++ -mp=gpu -gpu=cc80 -Ofast laplace2d_teams_distrib.cpp -o laplace_teams_distrib -Minfo=accel,mp
srun -p gpu --gres=gpu:1 --ntasks=1 --time=00:05:00 --mem=40G ./laplace_teams_distrib    
Result outside lab time: 8807.77 ms  

------
Same method for compiling and running laplace2d_teams_distrib_dataregion.cpp
Result outside lab time: 736.219 ms  

------
Same method for compiling and running laplace2d_teams_distrib_datareg_collapse.cpp
Result outside lab time: 395.138 ms  

------
Same method for compiling (but with gpu=cc80,managed) and running laplace2d_teams_distrib_datareg_collapse.cpp
Result outside lab time: 99.4909 ms  

--------
cp start solution:
0 residual 44699
100 residual 48209.3
200 residual 43324.4
300 residual 38697.1
400 residual 34302.6
500 residual 30124.1
600 residual 26153.1
700 residual 22383.1
800 residual 18807.5
900 residual 15418.8
22675.3ms
Temperature distribution:
Temperature at (1000, 0) = 663.29

------
cp final solution:
0 residual 44699
100 residual 48209.3
200 residual 43324.4
300 residual 38697.1
400 residual 34302.6
500 residual 30124.1
600 residual 26153.1
700 residual 22383.1
800 residual 18807.5
900 residual 15418.8
993.092ms
Temperature distribution:
Temperature at (1000, 0) = 663.29

----------
srun -p gpu --gres=gpu:1 --ntasks=1 --time=00:05:00 --mem=40G ./cfd_euler_unaltered
The process took 454 milliseconds. 

----------
nvc++ -mp=gpu -gpu=cc80 -Ofast cfd_euler.cpp -o cfd_euler -Minfo=accel,mp       
srun -p gpu --gres=gpu:1 --ntasks=1 --time=00:05:00 --mem=40G ./cfd_euler       
The results are correct.    
The entire process (WITH copying to and from) took 462 milliseconds.     
The calculation process on GPU (WITHOUT copying to and from) took 181 milliseconds.    




