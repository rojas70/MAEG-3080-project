# Monte Carlos Tree Search (MCTS) Tutorial

In this tutorial, you will learn:

-  The basics theory of MCTS
-  How to code a MCTS
-  Intuition behind MCTS (Why it works!)

We will build the knowledge upon the real application (Tic Tac Toe) to get intuitions about what, how and why MCTS works well in chess plays and other AI applications.


## Tic-Tac-Toe




Tic-Tac-Toe is a game that two players take turns playing on a three-by-three board. One player plays Xs and the other Os until one player wins by placing three marks in a row, horizontally, vertically, or diagonally.

Try this online game to get a feeling of Tic-Tac-Toe. [[link](https://playtictactoe.org/)]

<p align="center">
<img src="media/online-game.png" alt="drawing" width="300"/>
</p>

In this tutorial, we will try to develop a program (sotisficated AI) that will achieve human AI for the Tic-Tac-Toe Game.

## A Naive AI for solving Tic-Tac-Toe

Here we try to explain what is state. The state is the representation information to describe the current situation of environment. In the case of tic-tac-toe, the state is the current situation of the chess board. The following picture is three states in the chess game.

<!--![dsfdf](media/tic-tac-toe-states.png)-->
<p align="center">
<img src="media/tic-tac-toe-states.png" alt="drawing" width="400"/>
</p>

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

## Monte Carlos Tree Search for solving Tic-Tac-Toe

So far, you might feel that the naive approach might be too unrealistic. You are right. One of the most significant drawback is that the simulations grows with the number of all state-action pairs in the game. We can calculate the number of all state-action pairs for tic-tac toe as


<img src="https://render.githubusercontent.com/render/math?math=9*8*7...*1=9!=362880">

If you do 100 simulation for an action-state pair, you need to do 362880*100 simuations. That might be durable for some of the computers. But given a 10 by 10 board, the number of state-pair is fatorial of 100, 9.332622e+157, which is definitely out of computational power. In fact, given a n by n board, the number of state-pair is <img src="https://render.githubusercontent.com/render/math?math=n^2!">.

So is there some methods to create a more data-efficient AI? You know when professional players play a chess game, they will plan their moves a few steps ahead. In such method, they can try to foresee the resultant future states which an action might lead to. In AI theory, there is a similar method that is able to foresee the states. That is Monte Carlos Tree Search.

### Basics of Tree

Let us try to foresee by planning the move.

<p align="center">
<img src="media/tree.png" alt="drawing" width="400"/>
</p>

In the above picture, we illustrate the planning using a tree structure. Since the tree is large, only a part of the entire tree is shown in the picture.


