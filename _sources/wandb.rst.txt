Setup of W&B
============

Follow the instructions to install Weights and Biases visualization tools `here <https://docs.wandb.com/quickstart>`_. Weights and Biases is a visualization tool for ML projects. It allows for a clean, succinct way to show results, as well as a the ability to log vast amounts of different data types. 

To initialize our project with W&B, we use

.. code-block:: python

    import wandb
    wandb.init(project, config_dict)

where project is the project title and config_dict is a dictionary containing all run configurations.

To add a log call into RallyAI, use the command 

.. code-block:: python

    wandb.log(info_dict)
    
where info_dict is a dictionary containing all of the data and variables you wish to log.
