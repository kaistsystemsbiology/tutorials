DeepRFC
=======

.. automodule:: DeepRFC
    :members: 
    :undoc-members: 
    :show-inheritance: 

**DeepRFC running procedure**


-------------------------

**Note**: This source code was developed in Linux, and has been tested in Ubuntu 16.04 with Python 3.6. Anaconda was used as a package manager


1. Clone the repository

.. code-block::

        $ git clone https://bitbucket.org/kaistsystemsbiology/deeprfc.git


2. Create and activate a conda environment

.. code-block::

        $ conda env create -f environment.yml
        $ conda activate deeprfc

3. If pip error occurs due to Pytorch version,  
  Remove followings from *environment.yml*

  .. code-block::

        Torch==1.2.0+cu92
        Torchvision==0.4.0+cu02

  -Install an appropriate version of Pytorch, according to your computing envrionment.  
    Please refer to https://pytorch.org/get-started/locally/


*Example*

- Run DeepRFC

.. code-block::

        $ python deeprfc.py -i ./example/test.txt -o ./output 



------- 

