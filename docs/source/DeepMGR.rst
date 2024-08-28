DeepMGR
=======

.. automodule:: DeepMGR
    :members: 
    :undoc-members: 
    :show-inheritance: 

- Note: This source code was developed in Linux, and has been tested in Ubuntu 16.04 with Python 3.9

- Major dependency: rdkit


**Source**

1. Clone the repository

    .. code-block:: bash

        git clone https://bitbucket.org/kaistsystemsbiology/DeepMGR.git   


2. Create and activate a conda environment

    .. code-block:: bash

        conda env create -f environment.yml
        conda activate deepmgr

**Example**

- Run deepMGR with input files in './input'.

    .. code-block:: bash

        python run_deepmgr.py -i -s MG1655

- Run deepMGR without input files in './input'.

    .. code-block:: bash

        python run_deepmgr.py -b oxygen -c [Glucose,2] [Na2HPO4,12.8] [KH2PO4,3] [NaCl,0.5] [NH4Cl,1]
        [MgSO4,0.24] [CaCl2,0.0111] -s BW25113 -g b0001
- For more information:

    .. code-block:: bash

        python run_deepmgr.py -h


**Publication**
- Mun Su Kwon, Joshua Julio Adidjaja and Hyun Uk Kim. Predicting the effects of cultivation condition on gene expression levels in Escherichia coli by using deep learning.

------- 
