DeepDDI
=======

.. automodule:: DeepDDI
    :members: 
    :undoc-members: 
    :show-inheritance: 
    
    **DeepDDI running procedure**
    This project is to develop a framework that systematically predicts drug-drug interactions (DDIs) and drug-food interactions (DFIs).
    
    --------------------------

    **Features**
    - DeepDDI predicts DDIs and DFIs using names of drug-drug or drug-food constituent pairs and their structural information as inputs
    - DeepDDI predicts 86 DDI types as outputs of human-readable sentences

    **Note**: This source code was developed in Linux, and has been tested in Ubuntu 14.04

    1. Create and activate virtual environment
    
    .. code-block::
    
            $ virtualenv venv
            $ source venv/bin/activate

    2. Install packages at the root of the repository

    .. code-block::
    
            $ pip install pip --upgrade
            $ pip install -r requirements.txt

    3. Install [rdkit](http://www.rdkit.org/)

      In our case, we installed rdkit in the root of a server using apt-get (i.e., `apt-get install rdkit`), and created its symbolic link in `venv`:

    .. code-block::

          $ ln -s /usr/lib/python2.7/dist-packages/rdkit/ ./venv/lib/python2.7/site-packages/

    *Input files for the DeepDDI*
    
      Following working input files can be found in: `./data/`. These files were used for the data presented in the manuscript.

    *Example*
    
    .. code-block::
    
      $ python run_DeepDDI.py -i ./examples/input_structures.txt -o ./output_dir/ 
  

    
    ------- 
