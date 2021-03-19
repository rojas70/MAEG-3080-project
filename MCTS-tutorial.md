# Monte Carlos Tree Search (MCTS) Tutorial

In this tutorial, you will learn:

-  The basics theory of MCTS
-  How to code a MCTS
-  Intuition behind MCTS (Why it works!)

We will build the knowledge upon the real application (Tic Tac Toe) to get intuitions about what, how and why MCTS works well in chess plays and other AI applications.


## 1. Tic-Tac-Toe 


Tic-Tac-Toe is a game that two players take turns playing on a three-by-three board. One player plays Xs and the other Os until one player wins by placing three marks in a row, horizontally, vertically, or diagonally.

Try this online game to get a feeling of Tic-Tac-Toe. [[link](https://playtictactoe.org/)]

<p align="center">
<img src="media/online-game.png" alt="drawing" width="300"/>
</p>

In this tutorial, we will try to develop a program (sotisficated AI) that will achieve human AI for the Tic-Tac-Toe Game.

## 2. A Naive AI for solving Tic-Tac-Toe

Here we try to explain what is state. The state is the representation information to describe the current situation of environment. In the case of tic-tac-toe, the state is the current situation of the chess board. The following picture is three states in the chess game.

<!--![dsfdf](media/tic-tac-toe-states.png)-->
<p align="center">
<img src="media/tic-tac-toe-states.png" alt="drawing" width="400"/>
</p>

Note that the white "X" or "O" indicates the current turn for two players.

Given a state, we have one or multiple choices of available actions corresponding to the state. Let us take the first state in the above picture as example. The available actions can be listed as following:
<!--
![dsfdf](media/tic-tac-toe-case1.png)-->
<p align="center">
<img src="media/tic-tac-toe-case1.png" alt="drawing" width="400"/>
</p>

Player can put the "O"s to position 1 to 8 as the above picture shows. 

Ok, now if you are the player, which action among the available actions given this state will be the best or the smartest move?

Perhaps the most natural thing comes to your mind is to calculate the winning ratio of action given a particular state.
But how to calculate the winning ratio? Maybe one of the feasible ways is to do simulations to estimate and foresee the winning ratio of actions. 
Just like Doctor Strange!
<!--
![](media/doctor-strange.gif)
[<img src="media/doctor-strange.gif" width="250"/>](media/doctor-strange.gif)-->
<p align="center">
<img src="media/doctor-strange.gif" alt="drawing" width="400"/>
</p>

For example, given action 1, we can do hundreds of simulation to estimate the winning ratio. Eventually, you might calculate the winning ratios for states like this

<p align="center">
<img src="media/tic-tac-toe-action-winning-ratio.png" alt="drawing" width="400"/>
</p>

The number in black in the above picture is the probabity of winning ratio. Note that probabilities of all action sum to 1. Based on the result, you can make a "smart" moving, the one with the highest probability. That is Action 5, with probability 0.89, in the above picture.

## 3. Monte Carlos Tree Search for solving Tic-Tac-Toe

So far, you might feel that the naive approach might be too unrealistic. You are right. One of the most significant drawback is that the simulations grows with the number of all state-action pairs in the game. We can calculate the number of all state-action pairs for tic-tac toe as


<img src="https://render.githubusercontent.com/render/math?math=9*8*7...*1=9!=362880">

If you do 100 simulation for an action-state pair, you need to do 362880*100 simuations. That might be durable for some of the computers. But given a 10 by 10 board, the number of state-pair is fatorial of 100, 9.332622e+157, which is definitely out of computational power. In fact, given a n by n board, the number of state-pair is <img src="https://render.githubusercontent.com/render/math?math=n^2!">.

So is there some methods to create a more data-efficient AI? You know when professional players play a chess game, they will plan their moves a few steps ahead. In such method, they can try to foresee the resultant future states which an action might lead to. In AI theory, there is a similar method that is able to foresee the states. That is Monte Carlos Tree Search.

### 3.1 Basics of Tree

Let us try to foresee by planning the move.

<p align="center">
<img src="media/tree.png" alt="drawing" width="400"/>
</p>

In the above picture, we illustrate the planning using a tree structure. Since the tree is large, only a part of the entire tree is shown in the picture. To simplfy the representation of above picture, we use the following illustration, simply using nodes and edges.


<p align="center">
<img src="media/tree-representation.png" alt="drawing" width="800"/>
</p>

The node represent state and the edge represent available action. This allows us to analyse the resultant states of each action and its further consequence. Let us look deeper into the tree .

<p align="center">
<img src="media/tree-deep.png" alt="drawing" width="500"/>
</p>

As you might notice, each state has a blue number. The value means the winning probability of "O" given the current state (or the value in reinforcement learning). The above picture shows the deep tree with 3 steps looking ahead. 

### 3.2 How to traverse the tree <span style="color:blue">(Selection)</span>

So the next question might be how to jump from the parrent node to the child node. Since the blue number for each state means winning ratio of Player "O", the rules of tranverse is easily set as:

- Maximize the winning ratios of "O" at the turn of "O".
- Minimize the winning ratios of "O" at the turn of "X".

Such a rule is known as "MinMax", which is a famous algorithm in game theory.

<p align="center">
<img src="media/minmax.png" alt="drawing" width="500"/>
</p>

Why do we try to minimize the winning ratios of "O" at the turn of "X"? Because we try to make the player "X", the opponent of "O", be "smart" on his decision. An analogy I like to think about is that people will tend to play against the opponent is better or at least the same with their own for chess play to improve their skill of playing chess. In general, "MinMax" algorithm or the rules of tranverse can impose some contraints for explorations that finds the optimal policy.

### 3.3 How to expand or "grow" the tree <span style="color:blue">(Expansion)</span>

 After you tranverse the tree using "MinMax" algorithm, you will reach leaf nodes (the bottom of the tree). If you want to grow the tree to see a few more move ahead, you need to expand (or in other word "grow") the tree. It is easy to exand the tree as:
 
 - Add new nodes (representing new states) to the leaf nodes.
 - Initialize the nodes with initial values. In our case, just we initialze the value of winning ratio to zero.

<p align="center">
<img src="media/tree-grow.png" alt="drawing" width="500"/>
</p>

### 3.4 How to estimate the probability of winning ratio for a node 

From the perspective of frequentists, the probability of winning ration can be estimated using statistics, such as Monte Carlo methods. Let us check the following picture

<p align="center">
<img src="media/probability-calculation.png" alt="drawing" width="800"/>
</p>

n is the number of game plays starting at the current state, t is the number of winning plays starting at the current state. The estimate probability of wining ration is formulated as

<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=\hat{p} = t/m">
</p>

As the <img src="https://render.githubusercontent.com/render/math?math=n"> grows larger, the estimate probability get more accurate. So we might need to do simulations as many as possible to get an accurate winning ratio. Next we will explain how to increase the number of simulated plays.

### 3.5 Simulation<span style="color:blue">(Rollout)</span> 

For MCTS, after tranverse and expansion process, we will reach the leaf node. We will do rollout (or simulation in other word) when we find n=0.

<p align="center">
<img src="media/rollout.png" alt="drawing" width="400"/>
</p>

In the rollout, the learned policy will play againts a random policy, which might be the policy that give uniformly random moves. After the simulation, we record the result.



### 3.6 Update Result<span style="color:blue">(Backpropagation)</span> 

After a rollout, we will update the tree based on the result of rollout. 

<p align="center">
 <img src="media/backpropagation.png" alt="drawing" width="600"/>
</p>

As the above picture shows, we divide the cases into two:

- Case 1 (Player win): n and t are incremented by 1 for the rollout state, its parent state and its ancestral states.
- Case 2 (Player lose): n is incremented by 1 for the rollout state, its parent state and its ancestral states.

You might notice that the value is updated backward for the tree, which is similar to the backpropagation of gradient decent for neural neworks.


### 3.7 Full Procedure of MCTS 

<p align="center">
 <img src="media/mcts-process.png" alt="drawing" width="800"/>
</p>



