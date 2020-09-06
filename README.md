# Metaheuristic-project
Github repository for DSTI Metaheuristic project.

Hello,

This project has for goal to find optimisation solutions to the different problems of the exam. 
As it has been asked there is a file for each problem and this readme will describe the criteria used to choose the algorithms and their parameters.

Notes:
To do this exam, i have chose the "JMetalPy" package. To spare time i decided to use only this package so i could master it more easily. This package is not without weakness, for example: the "Simulated Annealing" algorithm documentation doen't provide a minimum temperature stopping criterion (if it exist i couldn't find it in the documentation), there is no possibility to export iterations data such as fitness in a file easily (!!only logged!! info are available) and the TSP class problem has to be modified to accept float coordinates.
But it does the job.

Discrete Optimization:

TSP Problems:
To use JMetalPy on this data base we have to modify the TSP problem class so it can accept float coordinates. Indeed, TSP file on the internet are usualy with integer coordinate and it looks like the developpers used "int()" to optimize the code.

So firt, we have to change the TSP class by creating another class that i called TSP2. The code is in the "TSP2 class file" (Master).

Then what algorithm to use? After some Simulated Algorithm experiments i decided to use Genetic Algorithm. Indeed, Simulated Annealing here is not so different than "trying every combinaison" because in JMetalPy it uses a mutation called "Swap Permutation" that select only 2 cities and permutate them. While in GA, if one of the parents have the optimal combinaison for some of the cities, his combinaison can survive threw the next generations, depending of the size of the offspring (childs) and the probability of crossover and mutation. Also, SA tend to convergence very quickly to a local optimum.
In term of performance, surprisingly, the GA seems to perform better than SA (according to the "ProgressBarObserver" object of JMetalPy) with around 2 000 iterations per seconds.


Djibouti data base:
This file is composed of 38 cities. By brute force we would have to compute 5,23e^44 different possibilities to find the optimal combinaison. I tried SA with tempreature = 500 and alpha = 0.999999. Best result was 6891 in 6 40 000 iterations and 354s.

