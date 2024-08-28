RetroPrecursorSelection
==============================

.. automodule:: RetroPrecursorSelection
    :members: 
    :undoc-members: 
    :show-inheritance: 
    
    **Retro-precursor-selection running procedure**
    
    ------

    **Note**
    This source code works on Linux and has been tested on Ubuntu 16.04 with Python 3.6.

    1. Clone the repository:
    
    .. code-block::

            $ git clone https://wdjang@bitbucket.org/kaistsystemsbiology/retro-precursor-selection.git

    2. Create and activate a conda environment (It takes less than 30 seconds):
    
    .. code-block::

            $ conda env create -f environment.yml
            $ conda activate SSA


    *Example*
    
    .. code-block::

            $ python run_ssa.py -i 'CCCCN'[SMILES of target product] -o output[Name of output directory]

    Three output files are generated in a folder that is newly created after computation: molecule_type.txt, predicted_precursors.txt, reaction_center.txt

    Run time of this source code is usually ~60 seconds.
    
    ------- 
