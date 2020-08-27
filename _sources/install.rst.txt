Installation
============

Download the following repositories to the same parent folder:

* `/Potential-Motors/RallyAI_v2 <https://github.com/Potential-Motors/RallyAI_v2>`_
* `/Potential-Motors/BasicSimulation <https://github.com/Potential-Motors/BasicSimulation>`_
* `/Potential-Motors/RallyAI_datasets <https://github.com/Potential-Motors/RallyAI_datasets>`_


Navigate to ``/RallyAI_v2/.``. These commands will install all necessary libraries and register the gym environment, respectively.

.. code-block:: bash

    pip install -r requirements.txt
    pip install -e .


We will make note that as of August 11th, 2020 we are using the bicycle simulation (python), so the following commands are unneccesary. To build the shared library for the simulation, navigate to ``BasicSimulation/C/`` and run

.. code-block:: bash

    make -f MakeShared
    sudo make -f MakeShared install


.. note:: You may also need to run ``apt-get install -y libsm6 libxext6 libxrender-dev``
