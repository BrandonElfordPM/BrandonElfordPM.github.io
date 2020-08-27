Environment
***********

The environment RallyAI is built within is based off of OpenAI's `Gym environments <https://gym.openai.com/docs/>`_. By doing this, we are able to use open source reinforcement learning packages like `stable baselines (TF1.14) <https://github.com/hill-a/stable-baselines>`_ or `stable baselines (PyTorch) <https://github.com/DLR-RM/stable-baselines3>`_. 

To define your own custom gym environment, you need to define five things

* Action Space
* Observation Space
* Step function
* Reset function
* Render function

The step function completed a single training step where RallyAI interacts with the environment. This includes updating observations and logging information.

The reset function resets the parameters to an initial state. In our case we have a soft reset that is used to reinitialized the vehicle, as well as a hard reset that is used between each episode. 

The render function can be used to visualize the steps taken by RallyAI during training, though we do this through our simulation which is not included in the render function as of yet.



Action Space
============
The action space defines the number of outputs from the neural network. We have created a schedule of learning goals, each with a defined test case. The initial test case has an action space of dimension 1, which we apply to the vehicle's steering angle. Specifically the requested steering angle, since we are limited by the motors reaction speed.

In later test cases, we may introduce additional actions like

* Torque
* Brake
* Multiple wheel controls
* etc



Observation Space
=================
The observation space defines the number of inputs to the neural network. As we attempt to optimize learning in each test case, we are adding/removing environment states to the observation space. For example, we have considered

* User inputs
* IMU data
* Desired states (advisor model)
* Distance to gate (gate model)
* etc

This is currently a very fluid aspect of RallyAI.


