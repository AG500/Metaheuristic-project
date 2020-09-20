# Metaheuristic-project
Github repository for DSTI Metaheuristic project.

Hello,

This project has for goal to find optimisation solutions to the different problems of the exam. 
As it has been asked there is a file for each problem and this readme will describe the criteria used to choose the algorithms and their parameters.

To do this project, i have chose the "JMetalPy v 1.5.5" package. To spare time i decided to use only this package so i could master it more easily. The only exception being for TSP where i also used Py2Opt package.
"JMetalpY" is not without weakness, for example: the "Simulated Annealing" algorithm documentation doen't provide a minimum temperature stopping criterion (if it exist i couldn't find it in the documentation), there is no possibility to export iterations data such as fitness in a file easily ("printed" data are available) and the TSP class problem has to be modified to accept float coordinates. But it does the job.

The processor used was an Intel core i5 (8th gen) and computations were made using only one core.

!!!!!!!Code and files!!!!!!

Discrete Optimization:

TSP Problems:
To use JMetalPy on this data base we have to modify the TSP problem class so it can accept float coordinates. Indeed, TSP file on the internet are usualy with integer coordinate and it looks like the developpers used "int()" to optimize the code.

So firt, we have to change the TSP class by creating another class that i called TSP2. The code is in the "TSP2 class file" (Master).

Then what algorithm to use? Because this is a signle objective combinatory problem, i tried both Simulated Algorithm and Genetic Algorithm. SA because it is a simple algorithm and i hoped than it would be much faster than other algorithms, and GA because of the analogy between DNA and the combination of cities.

Simulated Annealing here is not so different than "trying every combinaison" because in JMetalPy it uses a mutation called "Swap Permutation" that select only 2 cities and permutate them. While in GA, if one of the parents have the optimal combinaison for some of the cities, his combination can survive threw the next generations, depending of the size of the offspring (childs) and the probability of crossover and mutation. Also, SA tend to convergence very quickly to a local optimum while GA, with enough time, goes to the global optimum thanks to the mutation


Djibouti data base:
This file is composed of 38 cities. By brute force we would have to compute 5,23e^44 different possibilities to find the optimal combinaison. 

  Simulated Annealing :
I tried SA with temperature = 500 and alpha = 0.999999. The best result was 6 891 in 6 040 000 iterations and 354s. The temperature depend on the variations of the fitness during the computation here, because it converge quicly i chose 500 so that it can explore between 6 500 and 10 000. Because Djibouti is a relatively small problem, time is not an issue and because JMetalPy allow only the number of iterations as stopping criterion i chose a small alpha and a big number of iterations so that the SA algorithm could search a long time close to the global optimum.
    Convergence curve of SA on Djibouti TSP:
  ![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curve%20SA.JPG)



  Genetic Algorithm:
To keep at least the best individual of the last generation we have to chose use a numerous population, and a probability of mutation and of crossovers inferior to 1. But we have to keep a good number of computation. That is why after several tests i used these parameters : population : 10 000; mutation : 0.975; Crossover: 0.975. The global optimum has been found (fitness of 6 656)
Results are : 
fitness : 6 656;   time : 525 sec ; iterations : 3 900 000   
Solution: 8, 11, 10, 18, 17, 16, 15, 12, 14, 19, 22, 25, 24, 21, 23, 27, 26, 30, 35, 33, 32, 37, 36, 34, 31, 29, 28, 20, 13, 9, 0, 1, 3, 2, 4, 5, 6, 7.
  Convergence curve of GA on Djibouti TSP :
![alt text](


