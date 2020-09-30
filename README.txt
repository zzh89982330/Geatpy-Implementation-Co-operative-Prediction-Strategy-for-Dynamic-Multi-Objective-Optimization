Dependencies:

geatpy==2.2.3 (higher version may cause severe errors)
matplotlib>=3.0.0
numpy>=1.16.0
scipy>=1.0.0
cycler>=0.10
kiwisolver>=1.0.1
python-dateutil>=2.1

1. Project Structure:
The project code is mainly follow the architecture of the typical usage of geatpy, detailed documentation can be found at http://geatpy.com/index.php/home/
The core classes of the project are as follows:

    -ea.Problem: Problems which tends to solve (e.g. FDA2, FDA3, etc), it includes the parameters of the problem and the function for calculating
     the objective values given the decision variable
    -ea.Algorithm: This class defines the dynamic multi-objective optimization process, the proposed method and all comparing methods are defined using this class
    -ea.Population: Record all the information that the population tends to have, such as the objective matrix, decision variables

Class key Attribute and Function Details:
    
ea.Problem:
    ################Attributes###############
    -M: int; Number of dimensions
    -maxormins: array; type of optimization problem: maximize or minimize the objective, each component represents whether to maximize or minimize the corresponding
    objective
    -nt: int; adjust the changing magnitude of the optimization problem.
    -num_evaluations: int; nummber of generations before problem changes
    
    #################Functions################
    -aimFunc: function; function defining how the objective values are defined, the result will be assigned to given population's ObjV.
    -calBest: function; function defining how the theoretical pareto front (Objecive values) is calculated

ea.Population:
    ################Attributes###############
    -sizes: int; number of entities of the population
    -ObjV: matrix; N * M matrix representing the objective values, where N represents the population size, and M represents the number of optimization goals
    -Chrom: matrix; N * D matrix representing the decision variables, where N represents the population size, and D represents the dimension of decision variables

ea.Algorithm:
    #################Functions################
    -run: function; function defines the optimization algorithm, e.g. how to do the cross-over, mutation, and reinsertion and so on, the return value should be
    an optimized population.

2. How to change and run the code conducted in our paper?
    1) If only experiments are needed, except some tiny parameter changes may be made, no other changes are required. But some key issues are addressed below:
        a. 2D and 3D problems have different calculation methods such as the hyperplane calculation and the distance sorting method, thus they may not exchange codes
        between each other.
        b. The Concept Drift Condition Tests is based on the original 2D problems, but made changes to the Algorithm part which dynamicly changes the problem type or some
        parameters which tends to change only in Concept Drift tests, not the typical DMOP.

    2) Three core components(parameters) which we have different test settings are:
        a. Problem type: in the code, all problem types we use in paper are already defined, if user want to change the problem type tested, just change the variable "problem" in the 
        "Settings and Preparations" Section to the corresponding problem type
        b. Algorithm used: 

3. About the Concept Drift Problem Testing:
    The concept drift problem is modified from 2D problems for simplicity. The problem does not only change the optimization objective
    dynamicly, but also the changing pattern is also changing periodically which adds uncertainty and variations to the problem.
    This operation tends to compare the robustness of proposed method with other STOA methods under concept drift conditions
    Totally three files contains three concept drift senarios stated below:

    Generally, as stated in the paper, three kinds of concept drift is tested:
    1) Correlation Change: The changing pattern is changing, we dynamicly change to optimization problem (e.g. probelm switches between FDA2, FDA3, ZJZ)
    , every type of problem has a fixed number of time steps (e.g. 0-5 the problem is FDA2, 5-10 the problem is FDA3 and so on)
    
    2) Changing Magnitude: The nt parameter in the paper adjusts the changing magnitude the problem changes according to time, in our experiment, we adopt
    similar strategy with 1) (correlation change) that we change the nt parameter of the Problem class periodically

    3) Aggregation: The aggregation of changing magnitude and correlation change which changes both these two fields periodically.
    
    #################How to change the original settings?#############
    For compatibility, all the code logic are done in the Algorithm only, if user wants to test new optimization problems with concept drift, they need to implement and change the logic in
    Algorithm class. All other common parameters and settings can be changed according to last section about the typical Optimization problems
