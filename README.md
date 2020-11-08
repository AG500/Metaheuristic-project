# Metaheuristic-project
Github repository for DSTI Metaheuristic project.

Hello,

This project has for objective to find optimization solutions to the different problems of the exam. 
As it has been asked there is a file for each problem (8) and this readme will describe the criteria used to choose the algorithms and their parameters.

To do this project, i have chose the "JMetalPy v 1.5.5" package. To spare time i decided to use only this package so i could master it more easily. The only exception being for TSP where i also used Py2Opt package.
"JMetalPy" is not without weakness, for example: the "Simulated Annealing" algorithm documentation doesn't provide a minimum temperature stopping criterion (if it exist i couldn't find it in the documentation), there is no possibility to export iterations data such as fitness in a file easily ("printed" data are available) and the TSP class problem has to be modified to accept float coordinates. But it does the job.

The hardware used was an Intel core i5 (8th gen) with 8Go of RAM and Windows 10. Computations were made without multiprocessing. For every problem i decided not to exceed 4 hours of computation because of practical reasons and also because JMetalPy stopping criteria is only about a maximum number of iterations. I defined the maximum number of iterations of the most difficult problems with this rule. For most problem it is not an issue but for Schwefel's and Rosenbrock's function with D=500, the algorithm could need way more than 4 hours of computation. 

For every problem there is 3 files : the code of the problem's class, the code of the algorithm, and the convergence curve. To simplify, the algorithm code contain both D=50 and D=500 problems.

The running order is indicated in the name of the file. The problem class is always first and need only one run. For continuous problem it contains all the data needed, while for TSP you need to dowload the files. 

For the solutions, there is one file that contains all the solutions of the problems.

__How to run :__  

The class code has to precede the algorithm code. So you need to copy/paste the spécific class code of the problem, and the algorithm code to run it. It include the lower and upper bound and the data used for the problem.  
To change dimension (continuous problem only), the variable "number_of_variables" has to be changed inside the problem call.  
Example for D=500: problem = Sphere2(number_of_variables = 500)



## Discrete Optimization:

__TSP Problems:__  

To use JMetalPy on this data base we have to modify the TSP problem class of this package so it can accept float coordinates. Indeed, TSP file on the internet are usualy with integer coordinate and it looks like the developpers used "int()" to optimize the code.

So firt, we have to change the TSP class by creating another class that i called TSP2.

Then, what algorithm to use? Because this is a single objective combinatory problem, i tried both Simulated Algorithm and Genetic Algorithm. SA because it is a simple algorithm and i hoped than it would be much faster than other algorithms, and GA because of the analogy between DNA and the combination of cities.

Simulated Annealing here is not so different than "trying every combinaison" because in JMetalPy it uses a mutation called "Swap Permutation" that select only 2 cities and permutate them. While in GA, if one of the parents have the optimal combination for a part of the path, the combination can survive threw the next generations, depending of the size of the offspring (childs) and the probability of crossover and mutation. Also, SA tend to convergence very quickly to a local optimum while GA, with enough time, goes to the global optimum.


__Djibouti data base:__  

This file is composed of 38 cities. By brute force we would have to compute 5,23e^44 different possibilities to find the optimal combination. 

  __Simulated Annealing :__  

I tried SA with temperature = 500 and alpha = 0.999999. The best result was 6 891 with 6 040 000 iterations and 354s. The temperature depend on the variations of the fitness during the computation here, because it converge quickly i chose 500 so that it can explore between the fitness 6 500 and 10 000. Because Djibouti is a relatively small problem, time is not an issue and because JMetalPy allow only the number of iterations as stopping criterion i chose a small alpha and a big number of iterations so that the SA algorithm could search a long time close to the global optimum.
    Convergence curve of SA on Djibouti TSP:
  ![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Djibouti%20SA.JPG)



  __Genetic Algorithm:__  
  
To keep at least the best individual of the last generation we have to use a numerous population, and a probability of mutation and of crossovers inferior to 1. But we have to keep a good number of computation. After several tests i used these parameters : population : 10 000; mutation : 0.975; Crossover: 0.975. So, we can expect to have about 6 individuals with the same characteristics as their parents at each generation. The type of tournament here is a Binary Tournament (only the 2 best individuals are selected), which is enought. The mutation used is "SwapMutation" which permutate 2 cities at each iterations.

The global optimum has been found (according to Univ of Waterloo website the best fitness is 6 656).
Results are : 
   Fitness : 6 656;   time : 525 sec ; iterations : 3 900 000   
   Solution (0 being the first city): 8, 11, 10, 18, 17, 16, 15, 12, 14, 19, 22, 25, 24, 21, 23, 27, 26, 30, 35, 33, 32, 37, 36, 34, 31, 29, 28, 20, 13, 9, 0, 1, 3, 2, 4, 5, 6, 7.
  
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Djibouti%20GA.JPG)


__Qatar TSP:__  

This file has 194 cities, compared to Djibouti, this is a much more difficult challenge.

SA and GA approach:
The SA and GA algorithm with the parameters seen above are just too slow with my computer to find the optimum solution within days. So i propose to use the 2-opt algorithm.

The 2opt algorithm:
The 2opt algorithm provide a more efficent way to find candidates to the best solution. It is very sensible to the initial solution and converge quite quickly. So an improvement should be to used it as a "mutation" step for the GA for example. Replacing the "Swap Permutation" of JMetalPy by a "2-opt" mutation could increase the efficiency of the algorithm. Sadly, i couldn't try this kind of algorithm because of the complexity of the JMetalpy package and time. Beside, i tried the 2-opt algorithm using "Py2opt" package.

The "Py2opt" package need some modifications because it calculate the path without going back to the first city. The "calculate path" function of the "Solver" class has to be modified, i called it "Solver2". The class "RouteFinder" also need to be modified to call "Solver2" instead of "Solver". I called it "RouteFinder2".

This method converge quickly to a fitness around 10 000 but has trouble going under it. That's why i stop the algorithm at 500 iterations.
According to Univ of Waterloo website the best fitness is 9 362.
Results are:
  Fitness: 9 970;  time : 2633 sec; iterations: 500
  Solution (0 being the first city) : 0, 5, 7, 6, 10, 13, 12, 15, 22, 24, 16, 25, 23, 20, 17, 32, 27, 21, 28, 44, 56, 59, 68, 73, 71, 77, 74, 75, 86, 79, 70, 81, 61, 58, 35, 62, 19, 64, 84, 85, 97, 89, 88, 93, 98, 100, 103, 110, 113, 112, 108, 101, 102, 90, 92, 95, 105, 104, 106, 107, 115, 116, 117, 121, 118, 125, 124, 126, 129, 131, 133, 136, 139, 144, 141, 145, 148, 155, 160, 162, 163, 168, 171, 178, 175, 181, 193, 185, 182, 186, 189, 191, 190, 188, 187, 183, 176, 180, 192, 184, 179, 177, 167, 166, 161, 169, 170, 165, 159, 154, 157, 158, 164, 174, 172, 173, 156, 153, 137, 138, 140, 143, 149, 152, 151, 146, 150, 147, 142, 134, 135, 130, 128, 132, 127, 123, 122, 119, 120, 114, 111, 109, 99, 96, 94, 91, 87, 82, 78, 80, 83, 76, 69, 63, 67, 36, 26, 33, 39, 42, 46, 38, 50, 65, 72, 66, 60, 57, 55, 52, 51, 53, 47, 45, 40, 37, 43, 48, 54, 49, 41, 34, 30, 31, 29, 18, 14, 11, 9, 8, 4, 2, 3, 1

![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Qatar%20TSP%202opt%20algo.JPG)




## Continuous Optimization :

For every problem i used the JMetalPy package, using the "Sphere"  problem class of JM as a base for every functions. Because we have to use D=50 and D=500, i first try different combination of parameters with D=50 and take the best performance parameters as a starting point for D=500.


__Unimodal Functions:__

For unimodal function, diversity seems less important than intensification, here there is no risk for the algorithm to fall into a local optimum. So, the algorithm should focus in intensification. After several try with GA, SA, GDE, EA and PSO on the sphere function and the schwefel's function, i decided to focus on PSO wich seems more efficient for continuous functions. In my benchmarks, SA tend to go fast at the beginning of the run but doesn't reach the optimum and GA has about the same behavior but is more efficient. I think the mutation algorithm used in JMetalPy for these two algorithms explain that. It is called "PolynomialMutation" and it lacks precision when the algorithm goes close to the optimum. 

For D=500, because the computations are more computer intensive and take a long time i often try to reduce the size of the particle swarm as much as possible for quicker convergence. The stopping criteria is the number of iterations. I chose the iteration number depending of the time it took for the first iterations of a run. I accept the solution if the error with the global optimum is under 10e^-4.

Good to know with PSO : when C2 > C1, intensification is quick while when C2 < C1, intensification of the swarm is slow. A high W allow a good diversification but slow the convergence to an optimum (local or global). A big swarm is more computer itensive per iteration.

__F1 Shifted Sphere function:__

This is a simple function with no local optimum. Here i chose the parameters for a quick convergence. That's why i only have 3 particles for both D=50 and D=500, and w=0.
Here the performance of PSO are so good than fine tuning of the parameters is not necessary. 

D = 50:  
Parameters: Swarm Size = 3, C1 = 0.1, C2 = 1.5, w=0.  Iterations = 100 000.
Results : Fitness = -449.999999    Time : 23.9 sec. Time to be at -449.9999 :  5.6 sec
Solution : see solutions file

![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Sphere%20D%20%3D%2050.JPG)


D = 500:  
Parameters: Swarm Size = 3, C1 = 0.1, C2 = 1.5, w=0.  Iterations = 500 000.
Results : Fitness = -449.9999   Time : 23.9 sec. Time to be at -449.9999 :  5.6 sec
Solution : see solutions file

![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Sphere%20D%20%3D%20500.JPG)

  
  
__F2 Shifted Schwefel's function:__

Here the PSO algorithm of JMetalPy seems to have a lot of trouble with D=500. Regularly, the algorithm stop working for 10 min to 20 min and then restart. And sometimes, Python crash. I think it has to do with the memory allocation of Python/JMetalPy and the function "abs()" used for the function. The 8Go of RAM of my computer are quickly full with D=500. The consequence is, while with D=50 the optimum is find in less than 2 min, for D=500 the algorithm needs hours (if it doesn't crash). This is another reason why i set the size of the swarm at minimum (3). Without this phenomenon i am confident than the algorithm could reach the global optimum.

The parameters i chose are completely different of the Sphere Function. Here i noticed than  when C1 < C2 and C1 is big, it allow a quicker convergence to the optimum. The best compromise i found is the one i chose for D=50. The parameters of the previous Sphere function (C1<C2 with big C2) works well but is not as fast. When C1=C2 the performance are not as good.

D = 50:  
Parameters : Swarm Size = 3,  C1 = 1.5, C2 = 0.2, w=0.  Iterations = 1 000 000.  
Results : Fitness = -449;99995380281   Time : 175.37 sec. Time to be at -449.9999 :  112.79 sec  
Solution : see solutions file
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Schwefel%20D%20%3D%2050.JPG)

D = 500:  
Here we can see the "pauses" of the algorithm due to, i think, a memory management problem of JMetalPy. We can see several "flat" interval wich get longer and longer as the algorithm progress. The biggest gap is between 6 000 and 7 600 seconds.  
Parameters : Swarm Size = 3,  C1 = 1.5, C3 = 0.3, w=0.  Iterations = 2 500 000.  
Results : Fitness = -443.940909969242   Time : 8 508.40 sec. Time to be at -449.9999 :  not reached  
Solution : see solutions file
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Schwefel%20D%20%3D%20500.JPG)


__Multimodal function:__

__F3 Shifted Rosenbrock's function:__  
This problem is quite hard, algortihms tend to block at fitness = 480 and fitness = 430 for D=50. And below 430 the progression is very slow watever parameters i used. The dilema is to allow enough diversification to "cross" these two fitnesses but not so much so the convergence to the global optimum doesn't take to much time. Less particules we have, faster is the algorithm but it has more chances to be blocked at 480 or 430. When C2>1.2 and C1<C2 it often goes quickly to the fitness 430 but take time to go to 390. The performance of PSO on this function depend a lot of the first positions of the particules.

D = 50 :  
On this run the algorithm start from the fitness 138 916 236 and goes straight to 480 where it struggle to go throught. Sometimes the algorithm block at 430 as well but not this time. 
Parameters : Swarm Size = 15, C1 = 0.4, C2 = 1.6, w=0.1  Iterations = 6 000 000.
Results : Best fitness = 390.269571   Time : 2079,07 sec. Time to be at -390.9999 : not reached.  
Solution : see solutions file
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Rosenbrock%20D%20%3D%2050.JPG)

The parameters : Swarm Size = 7, C1 = 0.1, C2 = 1.5, w=0  Iterations = 6 000 000. Work also well with about the same results. But is trapped in an local optimum more easily.




D = 500 :  
I didn't find de the global optimum there, most of the time, with  0.1 <c1 <0.6 and 1.0<C2< 1.5 it goes well to the fitness around 1 200 and then has a very slow progression. When C1>C2 it often took too much time to convergence.







__F4 Shifted Rastrigin's function:__

There is a lot of local and narrow optimums here. Diversification is important, so the number of particles has to be high, and C1 > 0.5. Still, C2 need to be superior to assure a fast convergence to the global optimum. 

D = 50 :  
Parameters : Swarm Size = 20, C1 = 0.5, C2 = 1.3, w=0.1  Iterations = 1 000 000
Results : Best fitness = -329.999954   Time : 328.60 sec. Time to be at -329.9999 : 304.05 sec  
Solution : see solutions file
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Rastrigin%20D%3D50.JPG)


D = 500 :  
Parameters : Swarm Size = 20, C1 = 0.5, C2 = 1.3, w=0.1  Iterations = 5 000 000
Results : Best fitness = -326.015924   Time : 14 161.56 sec. Time to be at -329.9999 : not reached  
Solution : see solutions file
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Rastrigin%20D%3D500.JPG)


__F5 Shifted Griewank's function:__ 

The PSO algorithm is very efficient here and it goes quickly to the fitness -179. The difficulty is to find the parameters that can goes to -179.9999.
It doesn't require a lot of particles to find the optimum, less than 5 is enough and fast, with a C2=1.5 and C1 = 0.5.   
C2 + C3 shouldn't exceed 2 because when the algorithm approach to -179, too important mouvements of the particules make them miss the optimum and the algorithm take a lot of time to goes to -179.9999. W is set to 0.1 to assure some diversity.


D = 50:  
Parameters: Swarm Size = 5, C1 = 0.5, C2 = 1.5, w=0.3  Iterations = 100 000
Results : Best fitness = -179.99999892   Time : 26.57 sec. Time to be at -179.9999 :  8.92 sec  
Solution : see solutions file
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Griewank%20D%20%3D%2050.JPG)


D = 500: here w is only 0.1 and the number of particules is 3, theses parameters are better for D = 500 than D=50.   
Parameters: Swarm Size = 3, C1 = 0.5, C2 = 1.5, w=0.1  Iterations = 500 000.
Results : Best fitness = -179.99999932   Time : 1174.71 sec. Time to be at -179.9999 :  587.86 sec
Solution : see solutions file

![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Griewank%20D%20%3D%20500.JPG)

__F6 Shifted Ackley's function:__

This function has a "plateau" which mean diversification is important, but because there is one narrow global optimum, values of the parameters should not be too high or the algorithm could miss the optimum, and/or the convergence can be very long. I found the parameters C1 = 1 and C2 = 0.8 to be a good balance, then i set w=0 because it slows the convergence and is not useful.

D = 50:  
Parameters: Swarm Size = 5, C1 = 1, C2 = 0.8, w=0  Iterations = 300 000.
Results : Best fitness = -139.999963    Time : 71.94 sec. Time to be at -139.9999 :  51,03 sec  
Solution : see solutions file  
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Ackley%20D%20%3D%2050.JPG)

D = 500:  
Parameters: Swarm Size = 5, C1 = 1, C2 = 0.8, w=0  Iterations = 2 500 000.
Results : Best fitness = -139.999948    Time : 5243.39 sec. Time to be at -139.9999 :  4 034.94 sec  
Solution : see solutions file  
![alt text](https://github.com/AG500/Metaheuristic-project/blob/master/Convergence%20curves/Convergence%20curve%20Ackley%20D%20%3D%20500.JPG)



Thank you for reading.
