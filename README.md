# Metaheuristic-project
Github repository for DSTI Metaheuristic project.

Hello,

This project has for goal to find optimisation solutions to the different problems of the exam. 
As it has been asked there is a file for each problem and this readme will describe the criteria used to choose the algorithms and their parameters.

To do this project, i have chose the "JMetalPy v 1.5.5" package. To spare time i decided to use only this package so i could master it more easily. The only exception being for TSP where i also used Py2Opt package.
"JMetalPy" is not without weakness, for example: the "Simulated Annealing" algorithm documentation doen't provide a minimum temperature stopping criterion (if it exist i couldn't find it in the documentation), there is no possibility to export iterations data such as fitness in a file easily ("printed" data are available) and the TSP class problem has to be modified to accept float coordinates. But it does the job.

The processor used was an Intel core i5 (8th gen) and computations were made without multiprocessing.

For every problem there is 3 files : the code of the problem's class, the code of the algorithm, and the convergence curve. To simplify, the algorithm code contain both D=50 and D=500 problems. 

For the solutions, there is one file that contains all the solutions of the problems.

How to run : 
The class code has to precede the algorithm code. So you need to copy/paste the spécific class code of the problem, and the algorithm code to run it. The lower and upper bound and the data used for the problem.
To change dimension, the variable "number_of_variables" has to be changed inside the problem call. Ex for D=500: problem = Sphere2(number_of_variables = 500)



Discrete Optimization:

TSP Problems:
To use JMetalPy on this data base we have to modify the TSP problem class so it can accept float coordinates. Indeed, TSP file on the internet are usualy with integer coordinate and it looks like the developpers used "int()" to optimize the code.

So firt, we have to change the TSP class by creating another class that i called TSP2. The code is in the "TSP2 class file" (Master).

Then what algorithm to use? Because this is a single objective combinatory problem, i tried both Simulated Algorithm and Genetic Algorithm. SA because it is a simple algorithm and i hoped than it would be much faster than other algorithms, and GA because of the analogy between DNA and the combination of cities.

Simulated Annealing here is not so different than "trying every combinaison" because in JMetalPy it uses a mutation called "Swap Permutation" that select only 2 cities and permutate them. While in GA, if one of the parents have the optimal combination for a part of the path, the combination can survive threw the next generations, depending of the size of the offspring (childs) and the probability of crossover and mutation. Also, SA tend to convergence very quickly to a local optimum while GA, with enough time, goes to the global optimum.


Djibouti data base:
This file is composed of 38 cities. By brute force we would have to compute 5,23e^44 different possibilities to find the optimal combinaison. 

  Simulated Annealing :
I tried SA with temperature = 500 and alpha = 0.999999. The best result was 6 891 with 6 040 000 iterations and 354s. The temperature depend on the variations of the fitness during the computation here, because it converge quickly i chose 500 so that it can explore between the fitness 6 500 and 10 000. Because Djibouti is a relatively small problem, time is not an issue and because JMetalPy allow only the number of iterations as stopping criterion i chose a small alpha and a big number of iterations so that the SA algorithm could search a long time close to the global optimum.
    Convergence curve of SA on Djibouti TSP:
  ![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curve%20SA.JPG)



  Genetic Algorithm:
To keep at least the best individual of the last generation we have to use a numerous population, and a probability of mutation and of crossovers inferior to 1. But we have to keep a good number of computation. After several tests i used these parameters : population : 10 000; mutation : 0.975; Crossover: 0.975. So, we can expect to have about 6 individuals with the same characteristics as their parents at each generation. The type of tournament here is a Binary Tournament (only the 2 best individuals are selected), which is enought. The mutation used is "SwapMutation" which permutate 2 cities at each iterations.

The global optimum has been found (according to Univ of Waterloo website the best fitness is 6 656).
Results are : 
   Fitness : 6 656;   time : 525 sec ; iterations : 3 900 000   
   Solution (0 being thefirst city): 8, 11, 10, 18, 17, 16, 15, 12, 14, 19, 22, 25, 24, 21, 23, 27, 26, 30, 35, 33, 32, 37, 36, 34, 31, 29, 28, 20, 13, 9, 0, 1, 3, 2, 4, 5, 6, 7.

               Convergence curve of GA on Djibouti TSP :
  
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curve%20GA%20Djibouti.JPG)


Qatar TSP:
This file has 194 cities, compared to Djibouti, this is a much more difficult challenge.

SA and GA approach:
The SA and GA algorithm with the parameters seen above are just too slow with my computer to find the optimum solution within days. So i propose to use the 2-opt algorithm.

The 2opt algorithm:
The 2opt algorithm provide a more efficent way to find candidates to the best solution. It is very sensible to the initial solution and converge quite quickly. So an improvement should be to used it as a "mutation" step for the GA for example. Replacing the "Swap Permutation" of JMetalPy by a "2-opt" mutation could increase the efficiency of the algorithm. Sadly, i couldn't try this kind of algorithm because of the complexity of the JMetalpy package and time. Beside, i tried the 2-opt algorithm using "Py2opt" package.

The "Py2opt" package need some modifications because it calculate the path without going back to the first city. The "calculate path" function of the "Solver" class has to be modified, i called it "Solver2". The class "RouteFinder" also need to be modified to call "Solver2" instead of "Solver". I called it "RouteFinder2".

This method converge quicly to a fitness around 10 000 but has trouble going under it. That's why i stoped i did only 500 iterations.
According to Univ of Waterloo website the best fitness is 9 362.
Results are:
  Fitness: 9 970;  time : 2633 sec; iterations: 500
  Solution (0 being the first city) : 0, 5, 7, 6, 10, 13, 12, 15, 22, 24, 16, 25, 23, 20, 17, 32, 27, 21, 28, 44, 56, 59, 68, 73, 71, 77, 74, 75, 86, 79, 70, 81, 61, 58, 35, 62, 19, 64, 84, 85, 97, 89, 88, 93, 98, 100, 103, 110, 113, 112, 108, 101, 102, 90, 92, 95, 105, 104, 106, 107, 115, 116, 117, 121, 118, 125, 124, 126, 129, 131, 133, 136, 139, 144, 141, 145, 148, 155, 160, 162, 163, 168, 171, 178, 175, 181, 193, 185, 182, 186, 189, 191, 190, 188, 187, 183, 176, 180, 192, 184, 179, 177, 167, 166, 161, 169, 170, 165, 159, 154, 157, 158, 164, 174, 172, 173, 156, 153, 137, 138, 140, 143, 149, 152, 151, 146, 150, 147, 142, 134, 135, 130, 128, 132, 127, 123, 122, 119, 120, 114, 111, 109, 99, 96, 94, 91, 87, 82, 78, 80, 83, 76, 69, 63, 67, 36, 26, 33, 39, 42, 46, 38, 50, 65, 72, 66, 60, 57, 55, 52, 51, 53, 47, 45, 40, 37, 43, 48, 54, 49, 41, 34, 30, 31, 29, 18, 14, 11, 9, 8, 4, 2, 3, 1


Convergence curve of 2opt algorithm on Djibouti TSP :
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curve%20Qatar%20TSP%202opt%20algo.JPG)




Continuous Optimization :

For every problem i used the JMetalPy package, using the "Sphere"  problem class of JM as a base for every functions. Because we have to use D=50 and D=500, i first try different combinaision of parameters with D=50 and then use the same parameters as a base for D=500 wich is much more time consuming. 


Unimodal Functions:

For unimodal function, diversity seems less important than intensification, here there is no risk for the algorithm to fall into a local optimum. So, the algorithm should focus in intensification. After several try with GA, SA, GDE, EA and PSO i decided to focus on PSO wich seems more efficient. For D=500, because the computations are more computer intensive and take a long time i often try to reduce the size of the particle swarm as much as possible for quicker convergence. The stopping criteria is the number of iterations. I chose the iteration number depending of the time it took for D=50. I accept the solution if the error with the global optimum is under 10e^-4.

Goof to know with PSO : When C2 > C1, intensification is quick while when C2 < C1, intensification of the swarm is slow. A high W allow a good diversification but slow the convergence to an optimum (local or global). A big swarm is more computer itensive per iteration.

F1 Shifted Sphere function:
This is a simple function with no local optimum. Here i chose the parameters for a quick convergence. So a very low C1 and a high C2 with a very small swarm. I applied the same parameters for D=50 and D=500

D = 50
Parameters: Swarm Size = 3, C1 = 0.1, C2 = 1.5, w=0.  Iterations = 100 000.
Results : Fitness = -449.999999    Time : 23.9 sec. Time to be at -449.9999 :  5.6 sec
Solution : see solutions file

![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curve%20Sphere%20D%20%3D%2050.JPG)


D = 500
Parameters: Swarm Size = 3, C1 = 0.1, C2 = 1.5, w=0.  Iterations = 500 000.
Results : Fitness = -449.9999   Time : 23.9 sec. Time to be at -449.9999 :  5.6 sec
Solution : see solutions file

![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curve%20Sphere%20D%20%3D%20500.JPG)


F2 Shifted Schwefel's function:





Multimodal function:

With multimodal function there could be a lot of local optimum. Therefore diersification is important. With PSO the paramters C1, W, are more important than before.

F3 Shifted Rosenbrock's function:



F4 Shifted Rastrigin's function:
There is a lot of local and narrow optimums here. In order to get a quick convergence with a good diversification i chos after several tries  a big number of particles, w>0.2 and  C1 >0.4. I set C2 at 1, wich is about two times bigger than C1 to get a fast convergence.

D=50:



F5 Shifted Griewank's function:



F6 Shifted Ackley's function:

