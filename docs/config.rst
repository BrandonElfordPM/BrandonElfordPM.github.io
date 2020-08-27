Configuration
=============

Now that all necessary packages are installed, we will outline how to configure and tune your experiments.

The configuration file is located at ``RallyAI_v2/RL_Training/config/simple_train.json``. Inside the file you will see a dictionary of dictionaries, defining multiple variables which will affect the training. The tables below are different sections from this config file. 

+---------------------------------+------+-------+-----------------------------------------------------+
|   Param                         | Type | Range | Description                                         |
+=================================+======+=======+=====================================================+
| timesteps                       | int  |(0,inf)| Total number of timesteps for this training session |
+---------------------------------+------+-------+-----------------------------------------------------+
| num_agents                      | int  |(0,inf)| The number of agents running in parallel            |
+---------------------------------+------+-------+-----------------------------------------------------+
| randomize                       | bool |  N/A  | Defining if randomization is used for user states   |
+---------------------------------+------+-------+-----------------------------------------------------+
| run_on_aws                      | bool |  N/A  | Defines if we are running on AWS                    |
+---------------------------------+------+-------+-----------------------------------------------------+
| algorithm                       | str  |  N/A  | Determines what algorithm to use (stablebaselines)  |
+---------------------------------+------+-------+-----------------------------------------------------+
| load_model_path                 | str  |  N/A  | Location and name of zip file to load weights from  |
+---------------------------------+------+-------+-----------------------------------------------------+

The 'Network structure' table defines the architecture of the nueral network. Note that the param 'architecture' is a list which defines the dimension and number of hidden layers only.

+---------------------------------+-----------+-------+-----------------------------------------------------+
|   Param (Network structure)     | Type      | Range | Description                                         |
+=================================+===========+=======+=====================================================+
| policy_type                     | str       |  N/A  | Type of model type (i.e. "fcn", "lstm", etc)        |
+---------------------------------+-----------+-------+-----------------------------------------------------+
| architecture                    | list, int |(0,inf)| Structure of policy (hidden) layers                 |
+---------------------------------+-----------+-------+-----------------------------------------------------+
| lstm                            |           |  N/A  |                                                     |
+---------------------------------+-----------+-------+-----------------------------------------------------+
| policy_kwargs                   |           |  N/A  |                                                     |
+---------------------------------+-----------+-------+-----------------------------------------------------+

The 'Input source' table contains parameters regarding how the simulation/vehicle are controlled. The 'input_file' is the style of the file used to for controls. For example, "constant" will select a constant value for the user inputs.

+---------------------------------+------+----------------------+-----------------------------------------+
|   Param (Input source)          | Type | Range                | Description                             |
+=================================+======+======================+=========================================+
| input_file                      | str  |  N/A                 |                                         |
+---------------------------------+------+----------------------+-----------------------------------------+
| episode_length                  | int  | (0,inf)              | Number of steps or length of input file |
+---------------------------------+------+----------------------+-----------------------------------------+
| vehicle                         | str  | 'Newton' or 'GWagon' | Vehicle parameters used in simulation   |
+---------------------------------+------+----------------------+-----------------------------------------+

The WandB parameters are used for logging information to the Weights & Biases services.

+---------------------------------+------+-------+-----------------------------------------------------+
|   Param (WandB)                 | Type | Range | Description                                         |
+=================================+======+=======+=====================================================+
| wandb_params/log_to_wandb       | bool |  N/A  | Defines if the training data will be logged to W&B  |
+---------------------------------+------+-------+-----------------------------------------------------+
| wandb_params/log_frequency      | int  |(0,inf)| How frequently the data is sent to W&B              |
+---------------------------------+------+-------+-----------------------------------------------------+
| wandb_params/run_name           | str  |  N/A  | Name used for W&B run, also for saving weights      |
+---------------------------------+------+-------+-----------------------------------------------------+
| wandb_params/proj_name          | str  |  N/A  | Name used for W&B project, also for saving weights  |
+---------------------------------+------+-------+-----------------------------------------------------+
| wandb_params/time_code          | bool |  N/A  | Defines wether or not to time sections of code      |
+---------------------------------+------+-------+-----------------------------------------------------+

The below parameter directly relate with the algorithm that is chosen for training. Since we mostly use PPO, those params is what we have listed below. Additionally, the ranges given below are also only applicable for PPO. A more detailed explanation of the hyperparameters can be found `here <https://medium.com/aureliantactics/ppo-hyperparameters-and-ranges-6fc2d29bccbe>`_.

+---------------------------------+------+---------------+-----------------------------------------------------+
|   Param (Hyperparameters)       | Type | Range         | Description                                         |
+=================================+======+===============+=====================================================+
| hyperparams/learning_rate_start | float| [0.003, 5e-6] | Initial learning rate                               |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/learning_rate_end   | float| [0.003, 5e-6] | Final learning rate (less than or equal to init lr) |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/gamma               | float| [0.8,0.9997]  | Discount Factor Gamma                               |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/vf_coef             | float| [0.5,1]       | Value Function Coefficient Range                    |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/ent_coef            | float| [0,0.01]      | Entropy Coefficient                                 |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/lam                 | float| [0.9,1]       | GAE Parameter Lambda                                |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/cliprange           | float| {0.1,0.2,0.3} | Clipping                                            |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/noptepochs          | int  | [3,30]        | Epochs                                              |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/nminibatches        | int  | [4,4096]      | Minibatch                                           |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/n_steps             | int  | [32,5000]     | Horizon                                             |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/max_grad_norm       | float|               |                                                     |
+---------------------------------+------+---------------+-----------------------------------------------------+
| hyperparams/verbose             | int  | {0,1,2}       | Amount of detail in algorithm output                |
+---------------------------------+------+---------------+-----------------------------------------------------+

Lastly, the reward parameters are in the table below. The reward functions that are being tested currently are split into sections, with the weight of each model followed by the standard deviation (std) of each parameter used in said model.

+---------------------------------+------+-------+-----------------------------------------------------+
|   Param (Reward)                | Type | Range | Description                                         |
+=================================+======+=======+=====================================================+
| reward/w_torque                 | float| [0,1] | Weight of torque model in reward function           |
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/torque                   | list |[0,inf)| Std used in gaussian for torque                     |
+---------------------------------+------+-------+-----------------------------------------------------+
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/w_angle                  | float| [0,1] | Weight of angle model in reward function            |
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/angle                    | list |[0,inf)| Std used in gaussian for angle                      |
+---------------------------------+------+-------+-----------------------------------------------------+
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/w_advisor                | float| [0,1] | Weight of advisor model in reward function          |
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/advisor_yaw_std          | list |[0,inf)| Std used in gaussian for yaw                        |
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/advisor_vel_std          | list |[0,inf)| Std used in gaussian for velocity                   |
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/advisor_x_std            | list |[0,inf)| Std used in gaussian for X position difference      |
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/advisor_y_std            | list |[0,inf)| Std used in gaussian for Y position difference      |
+---------------------------------+------+-------+-----------------------------------------------------+
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/w_gates                  | float| [0,1] | Weight of gate model in reward function             |
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/gate_std_x               | list |[0,inf)| Std used in gaussian for X position difference      |
+---------------------------------+------+-------+-----------------------------------------------------+
| reward/gate_std_y               | list |[0,inf)| Std used in gaussian for Y position difference      |
+---------------------------------+------+-------+-----------------------------------------------------+

Once your confirguration file is as you like it, you can easily begin training RallyAI, which is described in the next section.



