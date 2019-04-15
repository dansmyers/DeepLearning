Genetic Algorithms with DEAP
============================

Install DEAP
------------
Install using conda.

```
conda install -c conda-forge deap
```


onemax.py
---------
Put the following code in a file named `onemax.py`. Run it and verify that it generates a vector of all 1's.

Look through the code and examine each section. Look at the DEAP toolbox functions and figure out what each one is doing.

```
#    This file is part of DEAP.
#
#    DEAP is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Lesser General Public License as
#    published by the Free Software Foundation, either version 3 of
#    the License, or (at your option) any later version.
#
#    DEAP is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
#    GNU Lesser General Public License for more details.
#
#    You should have received a copy of the GNU Lesser General Public
#    License along with DEAP. If not, see <http://www.gnu.org/licenses/>.


#    example which maximizes the sum of a list of integers
#    each of which can be 0 or 1

import random

from deap import base
from deap import creator
from deap import tools

creator.create("FitnessMax", base.Fitness, weights=(1.0,))
creator.create("Individual", list, fitness=creator.FitnessMax)

toolbox = base.Toolbox()

# Attribute generator 
#                      define 'attr_bool' to be an attribute ('gene')
#                      which corresponds to integers sampled uniformly
#                      from the range [0,1] (i.e. 0 or 1 with equal
#                      probability)
toolbox.register("attr_bool", random.randint, 0, 1)

# Structure initializers
#                         define 'individual' to be an individual
#                         consisting of 100 'attr_bool' elements ('genes')
toolbox.register("individual", tools.initRepeat, creator.Individual, 
    toolbox.attr_bool, 100)

# define the population to be a list of individuals
toolbox.register("population", tools.initRepeat, list, toolbox.individual)

# the goal ('fitness') function to be maximized
def evalOneMax(individual):
    return sum(individual),

#----------
# Operator registration
#----------
# register the goal / fitness function
toolbox.register("evaluate", evalOneMax)

# register the crossover operator
toolbox.register("mate", tools.cxTwoPoint)

# register a mutation operator with a probability to
# flip each attribute/gene of 0.05
toolbox.register("mutate", tools.mutFlipBit, indpb=0.01)

# operator for selecting individuals for breeding the next
# generation: each individual of the current generation
# is replaced by the 'fittest' (best) of three individuals
# drawn randomly from the current generation.
toolbox.register("select", tools.selTournament, tournsize=3)

#----------

def main():
    random.seed(64)

    # create an initial population of 300 individuals (where
    # each individual is a list of integers)
    pop = toolbox.population(n=300)

    # CXPB  is the probability with which two individuals
    #       are crossed
    #
    # MUTPB is the probability for mutating an individual
    #
    # NGEN  is the number of generations for which the
    #       evolution runs
    CXPB, MUTPB, NGEN = 1.0, 1.0, 40
    
    print("Start of evolution")
    
    # Evaluate the entire population
    fitnesses = list(map(toolbox.evaluate, pop))
    for ind, fit in zip(pop, fitnesses):
        ind.fitness.values = fit
    
    print("  Evaluated %i individuals" % len(pop))
    
    # Begin the evolution
    for g in range(NGEN):
        print("-- Generation %i --" % g)
        
        # Select the next generation individuals
        offspring = toolbox.select(pop, len(pop))
        # Clone the selected individuals
        offspring = list(map(toolbox.clone, offspring))
    
        # Apply crossover and mutation on the offspring
        for child1, child2 in zip(offspring[::2], offspring[1::2]):

            # cross two individuals with probability CXPB
            if random.random() < CXPB:
                toolbox.mate(child1, child2)

                # fitness values of the children
                # must be recalculated later
                del child1.fitness.values
                del child2.fitness.values

        for mutant in offspring:

            # mutate an individual with probability MUTPB
            if random.random() < MUTPB:
                toolbox.mutate(mutant)
                del mutant.fitness.values
    
        # Evaluate the individuals with an invalid fitness
        invalid_ind = [ind for ind in offspring if not ind.fitness.valid]
        fitnesses = map(toolbox.evaluate, invalid_ind)
        for ind, fit in zip(invalid_ind, fitnesses):
            ind.fitness.values = fit
        
        print("  Evaluated %i individuals" % len(invalid_ind))
        
        # The population is entirely replaced by the offspring
        pop[:] = offspring
        
        # Gather all the fitnesses in one list and print the stats
        fits = [ind.fitness.values[0] for ind in pop]
        
        length = len(pop)
        mean = sum(fits) / length
        sum2 = sum(x*x for x in fits)
        std = abs(sum2 / length - mean**2)**0.5
        
        print("  Min %s" % min(fits))
        print("  Max %s" % max(fits))
        print("  Avg %s" % mean)
        print("  Std %s" % std)
    
    print("-- End of (successful) evolution --")
    
    best_ind = tools.selBest(pop, 1)[0]
    print("Best individual is %s, %s" % (best_ind, best_ind.fitness.values))

if __name__ == "__main__":
    main()
```


knapsack.py
-----------
Copy onemax to create a new problem:

```
  shell$ cp onemax.py knapsack.py
```
   
Add the following lines to the top of the new file, right after the
import statements:

```
random.seed(0)
num_items = 10
capacity = num_items * 10
weights = [i + 1 for i in range(num_items)]
values = [i + 1 for i in range(num_items)]
random.shuffle(values)
```

This creates an instance of the knapsack problem with 10 items and
a capacity of 100.

How are the weights and values assigned?

Look for the line that starts with toolbox.register("individual" 
and change the last argument to the function to num_items.

Now, create a fitness function for the knapsack problem. Add 
a new function definition for evalKnapsack below the existing
evalOneMax function.

```
  def evalKnapsack(individual):
      """Fitness function for knapsack problem"""
```

The fitness function should add up the weights and values of all
the elements set to 1 in the input argument individual. If the 
weight is greater than capacity, it should return 0, otherwise
it returns the value of the items in the knapsack.

Note: you have to put a comma after any return value (this
technically makes it a tuple, which DEAP requires). For example,

```
  return 0,
```
  
Next, change the line toolbox.register("evaluate"... to use
evalKnapsack.

Run knapsack.py. Does your result make sense?


Testing
-------
Now increase num_items to 20. Does the result still make sense?

Try num_items = 50. What answer did you get? Compare your results
to your neighbors.

Now try num_items = 100. What happens?

Why does the GA fail to find any valid solutions for num_items = 100?
Think hard about this before moving on to the next section.


New Initialization
------------------
Modify the line that begins toolbox.register("attr_bool"... by
replacing the last three argumnets with one function:

```
  toolbox.register("attr_bool", rand_init)
```

Now write a new rand_init function that returns 1 with only a small
probability, like 10%.

Re-run the 100-item problem with this new initialization procedure.
Does it work?

Why did this change improve the algorithm?

Play with the other settings and see what the best result is that
you can obtain for a 100-value problem.


1000 Items
----------
Try a 1000-item problem. What happens?

First, try making the initial vectors sparser. Does this help?

Try making the population larger.

Now try reducing the mutation probability to something very small,
like .001. Why does this make a big difference?
