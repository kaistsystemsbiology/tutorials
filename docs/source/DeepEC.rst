DeepEC
======

.. automodule:: DeepEC 
    :members: 
    :undoc-members: 
    :show-inheritance: 

**DeepEC running procedure**

------

**Note**: 
Size of the protein sequence input file should be adjusted according to the memory size of your computer. 
This source code was developed in Linux, and has been tested in Ubuntu 14.04 with Python 2.7, Python 3.4, Python 3.5 or Python 3.6. 
It should be noted that Python 3.7 is currently not supported.



1. Clone the repository

.. code-block::


        $ git clone https://bitbucket.org/kaistsystemsbiology/deepec.git


2. Create and activate a conda environment


.. code-block::

        $ conda env create -f environment.yml
        $ conda activate deepec

3. Example


- Run DeepEC

.. code-block::

        $ python deepec.py -i ./example/test.fa -o ./output 


------- 
