Rewards and Costs
*****************
The current reward function(s) for RallyAI are defined below. Each method is defined further below.

Advisor Model
=============
When most of us learn to drive, we have an `expert` accompany us in the passenger seat. They may direct us how to control the car or correct us when we've made a mistake. This mechanism essentially makes up the advisor model.

In this method, we train using two models. One being RallyAI (who is learning to drive) while the other is an advisor (that is controlled deterministically through the user). The advisor model uses a simplified simulation, one without slip or outside forces that may act on RallyAI. This way, the expert controller can describe to our RallyAI the ideal state of the vehicle as it drives. Hopefully this leads to learning of control so that the AI can correct any extreme situation to match the `ideal case`. 

For example, imagine we are driving on an icy road and as we turn the wheels begin to slip. The advisor model, which contains no ice, would not slip and RallyAI would begin to notice a discrepency in it's current state and that of the advisor. We will denote the advisor's states as `desired states` Since we want the two models to have an identical state, RallyAI would begin measures to fix this.

The reward function for the advisor model is simply the sum of the normed differences of the current states and the desired states. The `state of the states` is still very fluid and the list of observations being considered is changing day by day. 

Additional Reward Models
========================
The following subsections describe orther reward functions or functions used in combination with the above reward function.

Gate Model
----------
The gate model involves running RallyAI and computing a gate we want the vehicle to pass through at time t. We use a 2 dimensional gate, eventually 3D, to compare the vehicles position in space to where we expect it. We use gaussian distribution to create a bump-shaped object in the road, where being closer to the centre of the bump returns larger rewards. The gate is updated based on the user's inputs and velocity of the vehicle in the previous timestep. 

Simplified User Map
-------------------
This simple reward function will be used as a precursor to the above complex reward function. It is based on the difference in user/driver input and wheel states. We also shift from punishment/cost to only positive rewards by taking the gaussian (denoted :math:`\mathbb{N}`) of the difference. Note that :math:`||x||_2` is the Euclidean norm.

The twelve wheel states (4x[torque, brake and steering angle]) are used in the reward function, as well as the three user inputs (throttle, brake pedal and steering wheel). 

The reward function is below.

.. math:: R(t) = c_t\cdot torque_{diff} + c_b\cdot brake_{diff} + c_s\cdot steer_{diff}

where :math:`c_t,c_b` and :math:`c_s` are constants. Below we define the variables in the simple reward function.

.. math:: 

    torque_{diff} &= \mathbb{N}(||usr_{thr}-fl_{tor}||_2)+\mathbb{N}(||usr_{thr}-fr_{tor}||_2)+\mathbb{N}(||usr_{thr}-rl_{tor}||_2)+\mathbb{N}(||usr_{thr}-rr_{tor}||_2
    
    brake_{diff} &= \mathbb{N}(||usr_b-fl_b||_2)+\mathbb{N}(||usr_b-fr_b||_2)+\mathbb{N}(||usr_b-rl_b||_2)+\mathbb{N}(||usr_b-rr_b||_2)
    
    steer_{diff} &= \mathbb{N}(||usr_{sa}-fl_{sa}||_2)+\mathbb{N}(||usr_{sa}-fr_{sa}||_2)+\mathbb{N}(||usr_{sa}-rl_{sa}||_2)+\mathbb{N}(||usr_{sa}-rr_{sa}||_2)
    
    
    
Magnified Saltatory Reward
--------------------------
Defined `here <https://www.hindawi.com/journals/mpe/2019/7619483/>`_, the MSR algorithm magnifies *good* and *bad* rewards computed by the other function components. MSR is applied after a reward is computed, and means to accelerate positive learning. If a move results in a reward that is sufficiently better, or worse, than the previous reward, that reward is magnified to produce more good moves, and less bad moves. The algorithm is outlined in pseudo-code `here <https://www.hindawi.com/journals/mpe/2019/7619483/alg1/>`_.

The threshold of :math:`\nu` must be explored to find the value which results in the best learning.

This was attempted by Brandon earlier, but in a discrete way, whereas this method is continuous and fits better with our environment space. 



Catastrophic Control Failures
-----------------------------
Since our top priority is safety, we want to make sure the vehicle stays away from catastrophic failures, such as crashing, swerving into the other lane or leaving the road entirely. One of the ways to prevent such behaviour in an AI is by punishing the vehicle with a large negative reward. 
