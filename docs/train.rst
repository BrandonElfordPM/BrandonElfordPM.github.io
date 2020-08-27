Training
--------

To train RallyAI, we load the configuration parameters, build our custom gym environment and use the simulation and learning algorithm to train the policy network based off the reward function. To begin this process, navigate to ``/RallyAI_v2/RL_Training/`` and run

.. code-block:: bash

    python train.py
    
    
The train logs the information to W&B based off of your project name and run name. 


========================
Configuration parameters
========================

This is a list of the main paramenter in the configuration JSON file. Some of these parameters are also a dictionary, and will be defined in the following sections.

.. list-table:: Main parameters on config file
	:widths: 30 70
	:header-rows: 1

	* - Parameter
	  - Meaning
	* - timesteps
	  - Training time steps
	* - num_agents
	  - Number of agents to train with
	* - network_structure 
	  - Parameters configuring the neural network type and structure
	* - input_source
	  - Parameters configuring the input source (constant or file)
	* - randomize
	  - Apply domain randomization
	* - run_on_aws 
	  - Run on aws (log to console instead of wandb)
	* - wandb_params
	  - Parameters for logging to WandB
	* - algorithm
	  - Training algorithm: "ppo2", "a2c" or "acktr"
	* - hyper_params
	  - Training algorithm-related hyper parameters. "null" for default values.
	* - reward 
	  - Reward-building
	* - load_model_path
	  - Path to existing model for re-training. "null" to create a new model.


----------------------------
network_structure dictionary
----------------------------

You can configure your Neural Network structure inside this dictionary. There are some policy types available currently. The number of hidden layers, and the number of units on each hidden layer are also configured here.

.. list-table:: network_structure dictionary
	:widths: 30 70
	:header-rows: 1

	* - Parameter
	  - Meaning
	* - policy_type
	  - NN structure to use (fcn, MlpPolicy)
	* - architecture 
	  - Size of hidden layers in fcn or MlpPolicies policies
	* - lstm 
	  - Size of lstm layer. None if you don't want this layer


-----------------------
input_source dictionary
-----------------------

Define where we will get the user input from. Currently we have two modes: input from file and constant input. If you want to use a file, set its name (full path) on *input_file* parameter. For constant input, set *input_file* to "constant" and configure the constant input on *constant_torque*, *constant_brake* and *constant_angle* parameters.

This dictionary let you configure the initial parameters of the simulation too.

.. list-table:: input_source dictionary
	:widths: 30 70
	:header-rows: 1

	* - Parameter
	  - Meaning
	* - input_file 
	  - File to read the input from, "constant" for using a constant value instead
	* - episode_length
	  - null for using file length as episode length. Otherwise, set file length here
	* - vehicle
	  - Newton or Gwagon - different configuration
	* - initial_velocity
	  - Initial velocity on simulation
	* - constant_velocity
	  - True for constant velocity on simulation (ignore torque)
	* - constant_torque
	  - When "constant" input, this is the value of torque
	* - constant brake
	  - When "constant" input, this is the value of brake
	* - constant_angle 
	  - When "constant" input, this is the value of angle


-----------------------
wandb_params dictionary
-----------------------

Logs your current training information to Weights and Biases 'https://www.wandb.com/'_.

*NOTE*: You can configure the run name in this dictionary. However, if you need a fuller name, containing every configuration of your current training, set the variable *GEN_RUN_NAME* to True inside common/model.py.

.. list-table:: wandb_params dictionary
	:widths: 30 70
	:header-rows: 1

	* - Parameter
	  - Meaning
	* - log_to_wandb
	  - True for logging to WandB
	* - log_frequency
	  - Log every N time steps
	* - run_name 
	  - Run name inside WandB
	* - proj_name 
	  - Project name inside WandB


-----------------
reward dictionary
-----------------

You can build your own reward by combining the following options:

* **MSR** Magnify Saltatory Reward 'https://www.hindawi.com/journals/mpe/2019/7619483/'_
* **Torque mapping**. Map throttle user input to the action proposed by the Neural Network.
* **Angle mapping**. Map angle user input to the action proposed by the Neural Network.
* **Advisor reward**. Use our additional simulation as advisor to compute the reward function. Here you can build your own advisor reward by combining a set of state mappings.
* **Gate reward**. Use a moving checkpoint to reward the agent when getting closer to it.

*NOTE*: Every standard deviation in the following table can either be a floating number or a list of numbers. When using a list of numbers, Curriculum Learning (CL) will be applied. CL means that when the agent reaches a certain reward threshold, the next standard deviation will be applied, making the problem slightly harder and therefore, fine tuning our current results.


.. list-table:: reward dictionary
	:widths: 30 70
	:header-rows: 1

	* - reward
	  - Meaning
	* - msr
	  - true for applying MSR to your reward. False otherwise
	* - epsilon
	  - For MSR, the value of epsilon (it is commonly a very small value)
	* - nu
	  - For MSR, the value of nu (2, 3, 4, 5 are common values)
	* - torque 
	  - initial standard deviation for torque map
	* - w_torque
	  - weight for torque map in the reward function
	* - angle 
	  - initial standard deviation for angle map
	* - w_angle
	  - weight for angle map in the reward function
	* - w_advisor 
	  - weight for the advisor reward function
	* - advisor_x_std
	  - Standard deviation for accurately mimicking advisor's x position
	* - advisor_y_std
	  - Standard deviation for accurately mimicking advisor's y position
	* - advisor_yaw_std
	  - Standard deviation for accurately mimicking advisor's yaw value
	* - advisor_vel_std
	  - Standard deviation for accurately mimicking advisor's velocity
	* - advisor_accel_std 
	  - Standard deviation for accurately mimicking advisor's acceleration
	* - advisor xdot_std
	  - Standard deviation for accurately mimicking advisor's change in x position
	* - advisor ydot_std
	  - Standard deviation for accurately mimicking advisor's change in y position
	* - advisor yawdot_std
	  - Standard deviation for accurately mimicking advisor's change in yaw
	* - advisor_x_weight
	  - Weight for the advisor's X position
	* - advisor_y_weight
	  - Weight for the advisor's Y position
	* - advisor_yaw_weight
	  - Weight for the advisor's yaw
	* - advisor_vel_weight
	  - Weight for the advisor's velocity
	* - advisor_accel_weight
	  - Weight for the advisor's acceleration
	* - advisor_xdot_weight
	  - Weight for the advisor's change in X position
	* - advisor_ydot_weight
	  - Weight for the advisor's change in Y position
	* - advisor_yawdot_weight
	  - Weight for the advisor's change in yaw
	* - w_gate
	  - weight for the checkpoints reward function
	* - gate_std_x
	  - Standard deviation for accurately passing the checkpoint in X positon
	* - gate_std_y
	  - Standard deviation for accurately passing the checkpoint in Y position

-----------------------
hyper-params dictionary
-----------------------

Hyper parameters depend on the algorithm you chose to train. Check stable baselines documentation for a deeper explanation. 'https://stable-baselines.readthedocs.io/en/master/'_
