# Classical Planning (PDDL) Method

Planning Domain Deﬁnition Language (PDDL) is currently a widely recognised method in the Artificial Intelligence field. In this project, we use PDDL to define our planning problem and its respective response. We will show our motivation, application, trade-offs, and future improvements of the PDDL approach in this section.

# Table of Contents
- [Classical Planning (PDDL)](#governing-strategy-tree)
  * [Motivation](#motivation)
  * [Application](#application)
  * [Trade-offs](#trade-offs)     
     - [Advantages](#advantages)
     - [Disadvantages](#disadvantages)
  * [Future improvements](#future-improvements)

## Governing Strategy Tree  

### Motivation  
We chose PDDL because of its powerful declarative nature, which allows for defining a wide range of domains and problems. We wanted to focus on higher-level strategies while using automated planners (FF) to handle specific details depending on the situation.

The main idea is to have agents implement offensive and defensive behaviors in Pacman. 

Our attack aim is for the agents to enter the opponent's borders and survive the opponent's ghosts to eat their food. Our goal is for agents to enter the opponent's boundary and eat their food while successfully moving through the opponent's ghosts. To eat food optimally and survive the ghost hunt, the agent will use the help of a classical planning solver, which will solve problems in specific situations. In addition, the agents will respond to different situations, which will help extend the survival time and carry more food home successfully, reducing the chances of food loss.

For defense, our main aim is to protect the food as much as possible to prevent them from the enemies eating and bringing food back to their territory. The agents use a classical planning solver and a well-defined domain to explicitly define the problem in a given situation, thus protecting the food from being eaten by the enemies. Additionally, the agents take situation-specific actions such as eating the enemies to better protect the food and reduce the chance of them scoring points.

[Back to top](#table-of-contents)

### Application  

We use different init and goal predicates for our problem by current game state and control strategy. There are also different strategies for offensive and defensive agents. When a strategy is decided on, objects, initial and goal predicates for the problem are created, and a file is generated by the problem generator. The problem solver will design a plan based on the PDDL problem file. Finally, it passes it to the FF plan parser for the next best action.

#### Attack
The decision tree below describes the strategy of our offensive agent. Its main goals are to eat the nearest food, eat the capsule, get home safely, and actively die if it cannot get home.
![Attack DT](https://github.com/Yuqi-D/Classical-Planning-for-Pacman/blob/main/img/M1.png)

We use the following well-defined domain to generate problems via the game state.
| Action | Precondition | Effect |
| --- | --- | --- |
| `move` | No enemies at the connected destination | Agent moves `from` -> `to` |
| `move-non-limit` | The path to destination is connected | Agent moves `from` -> `to` |
| `move-with-capsule` | Agent moves to the connected destination after capsule disappeared | Agent moves `from` -> `to` |
| `eat-food` | Agent and the food are in the same position | Agent carryies the food |
| `eat-capsule` | Agent and the capsule are in the same position | Agent eats the capsule |
| `death` | Agent and the ghost are in the same position | Agent died and lose all the carring food |


#### Defend
The decision tree below describes the strategy of our defensive agent. Its main goals are to protect the capsule, eat the enemies, go to the latest place where food disappeared (the enemy ate) to find the enemy and protect the foods near the boundary.
![Defend DT](https://github.com/Yuqi-D/Classical-Planning-for-Pacman/blob/main/img/M1_2.png)

We use the following domain to generate problems via the game state.
| Action | Precondition | Effect |
| --- | --- | --- |
| `move` | The path to destination is connected | Ghost moves `from` -> `to` |
| `eat-invader` | Invader and the ghost are in the same position | Ghost eats the invader |

The performance of this method is good, but there is still something to improve.
![](https://github.com/Yuqi-D/Classical-Planning-for-Pacman/blob/main/img/M1_3.png)

[Back to top](#table-of-contents)

### Trade-offs  
#### *Advantages*  

As the game evolves, we can easily modify and extend the description and definition of the problem for different situations. When we define the game (defence and attack) in the domain, it is easy to dynamically generate problem files as the game progresses. The planner then returns the plan that best fits these situations. In addition, when the planner finds the optimal path, it does not need to explicitly code to set the goals. For example, for the agents' choice to eat food, we make all food as objects in the problem, and the planner automatically returns the path to the nearest food.

#### *Disadvantages*

One huge disadvantage of the PDDL approach is that it is difficult to predict the future and act on it. This leads to the possibility of our agents being chased into the places that are difficult to get out of (three walls around them).

[Back to top](#table-of-contents)

### Future improvements  

Classical planning can be improved by adding more predicates about the expected future state of the game. Since classical planning requires a static problem statement, we can extend the search space for the classical planning problem solver by improving the generation of problems that can include complex objects and fluents.

[Back to top](#table-of-contents)
