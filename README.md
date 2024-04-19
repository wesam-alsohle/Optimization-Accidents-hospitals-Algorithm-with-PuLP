# Optimization-Accidents-hospitals-Algorithm-with-PuLP

### Introduction:
Finding the optimal solution can be divided into five general steps:
• Describing the problem.
• Gathering the data.
• Formulating the mathematical program.
• Solving the mathematical program.
• Performing some analysis and modifying the solution multiple times.
• Presenting and analyzing the solution.

### Data:
Data has been collected regarding accidents and hospitals in London, containing the following attributes:

1. id_origin: A unique identifier for the accident.
2. id_destination: A unique identifier for the hospital.
3. severity: A number representing the severity of the accident.
4. Distance in meters: The distance between the accident and the hospital.
5. Duration in seconds: The time taken to travel from the hospital to the accident without considering traffic congestion.
6. Duration in traffic: The time taken to travel from the hospital to the accident, taking into account traffic congestion.
7. Duration return: The time taken to return from the accident to the hospital without considering traffic congestion.
8. Return duration with traffic: The time taken to return from the accident to the hospital, considering traffic congestion.

### Optimal Solution Algorithms:
We have four fundamental problems that we want to solve, but there are common steps to reach the hospital in the event of a specific accident at the lowest possible cost:
For each accident:
1- We traverse the nearest five hospitals.
2- Define the objective function of the problem, which is to obtain the lowest possible cost to reach the accident and then return to the appropriate hospital.
3- Create decision variables that will give us the number representing the suitable hospital when solving the problem.
4- Formulate the objective function with a mathematical equation where a cost is assigned to each feature indicating its importance.
5- Formulate the problem constraints, which in our case are: the decision variable values must be greater than zero, and only one hospital must be selected, where the decision variable value equals one.
6- Solve each problem using three algorithms: ['GLPK_CMD', 'GUROBI', 'PULP_CBC_CMD']
7- Filter the proposed solutions based on the most important feature of the current problem.
8- Present the optimal solution.


Note: The difference between the four problem scenarios lies solely in the formulation of the objective function and the assignment for the current scenario. For the severity feature, its values have been reversed so that for high severity, the cost becomes low. This is to solve the entire problem as a Minimization problem.

#### Firstly, for the problem of traveling from the hospital to the accident and returning to the same hospital without considering traffic congestion:

Regarding the objective function, the total cost is determined by summing the values of the following features, with appropriate weights assigned to each to make the overall cost equal to:

Total Cost = (Distance between hospital and accident * 2) + (Severity of the accident * 1) + (Duration of travel to the accident without congestion * 10) + (Duration of return to the same hospital without congestion * 10)


#### Secondly, for the problem of traveling from the hospital to the accident and returning to the same hospital while considering traffic congestion:

Regarding the objective function, the total cost is determined by summing the values of the following features, with appropriate costs assigned to each to make the overall cost equal to:

Total Cost = (Distance between hospital and accident * 2) + (Severity of the accident * 1) + (Duration of travel to the accident with congestion * 10) + (Duration of return to the same hospital with congestion * 10)

#### Discussion:
✓ Multiplying the distance by a weight value of "2" is because we are both going to and returning from the same hospital, so we cover the same distance both ways. Therefore, the total distance is the distance traveled to the accident.
✓ Multiplying the time by a weight value of "10" is to ensure that their range of values is determined independently of the distance values because their values themselves will not affect the total cost.
✓ In case the total costs for each accident are close due to the sum of distances and times multiplied by their weights, we differentiate them by adding a weight indicating the severity of the accident. Consequently, the cost will be higher for the more severe accidents by multiplying their values by two distances.
✓ The algorithm will provide the most suitable hospital to go to and the most suitable hospital to return to, and the cost will be fair when selecting the hospital in terms of time and distance.

#### Thirdly, for the problem of going from the hospital to the accident and returning to any hospital without considering traffic congestion:

Regarding the objective function, the total cost is determined by summing the values of the following features, with appropriate costs assigned to each to make the cost of going from the hospital to the accident equal to the total distance between the hospital and the accident plus the severity of the accident multiplied by the duration of travel to the hospital without congestion multiplied by 10. Similarly, the cost of returning from the accident to any hospital equals the total distance between the hospital and the accident plus the severity of the accident multiplied by the duration of return to the hospital without congestion multiplied by 10.


#### Fourthly, for the problem of going from the hospital to the accident and returning necessarily to the same hospital while considering traffic congestion:

Regarding the objective function, the total cost is determined by summing the values of the following features, with appropriate costs assigned to each to make the cost of going from the hospital to the accident equal to the total distance between the hospital and the accident plus the severity of the accident multiplied by the duration of travel to the hospital with congestion multiplied by 10. Similarly, the cost of returning from the accident to the hospital equals the total distance between the hospital and the accident plus the severity of the accident multiplied by the duration of return to the hospital without congestion multiplied by 10.


## Results and Interpretation:
The three optimal solution selection algorithms were tested on the four problems: ['GLPK_CMD', 'GUROBI', 'PULP_CBC_CMD']. The result was that they all yielded the same total cost. To simplify complexity and reduce solution time, the 'PULP_CBC_CMD' algorithm was selected as it provided similar results with less computational burden.

| Total Total Cost | Issue |
|-----------------------|---------|
| 1- Go and return to the same hospital with congestion| 22352410.0|
| 2- Go and return to the same hospital without congestion| 22736288.0|
| 3-1 Go to a hospital with congestion| 11124259.0|
| 3-2 The return is not necessarily to the same hospital with congestion| 10962408.0|
| 3- The total of going and returning is not necessarily to the same hospital with congestion| 22086667.0|
| 4-1 Going relative to a hospital without congestion| 11385344.0|
| 4-2 The return is not necessarily to the same hospital without congestion| 11087340.0|
| 4- The total of going and returning is not necessarily to the same hospital with congestion| 22472684.0|

### Comparing the four issues, we find that the case of going to and returning from the same hospital is the most suitable scenario when an accident occurs. We observe that the difference is minor between the congested and non-congested scenarios.
