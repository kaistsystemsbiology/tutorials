ReconManager 
===============

.. automodule:: ReconManager 
    :members: 
    :undoc-members: 
    :show-inheritance: 
    
    **Recon-manager procedure**
    
    This project is to develop a framework that systematically predicts Gene-Transcript-Protein-Reaction Associations (GeTPRA) in human metabolims and updates a human genome-scale metabolic model (GEM) accordingly. **Recon manager** is a part of the project, which is a collection of scripts to generate Recon 2M.1 and simulate Recon models.
    
    ------
    

    **Features**
    **Recon manager** contains scripts that implement following tasks independently:

    - Convert GPR to TPR associations
    - Update metabolite information
    - Calculate model statistics
    - Evaluate functionality of metabolic model
    - Reconstruct personal GEMs using tINIT

    **Installation**
    *Major dependencies*
    - [gurobipy](http://www.gurobi.com/)

    *Procedure*
    **Note**: This source code was developed in Linux, and has been tested in Ubuntu 14.04.5 LTS (i7-4770 CPU @ 3.40GHz)

    1. Clone the repository

    2. Create and activate virtual environment
    
    .. code-block::
    
            $ virtualenv venv
            $ source venv/bin/activate

    3. Install packages at the root of the repository

    .. code-block::
    
            $ pip install pip --upgrade
            $ pip install -r requirements.txt

    4. Install [gurobipy](http://www.gurobi.com/)

        In our case, we installed gurobipy in the root of a server, and created its symbolic link in `venv`:

        .. code-block::
        
            $ ln -s /usr/local/lib/python2.7/dist-packages/gurobipy/ $HOME/recon-manager/venv/lib/python2.7/site-packages/

    *Feature: Convert GPR to TPR associations*
    Input arguments and corresponding files
    Following working input files can be found in: `./input_data/GPR_to_TPR_inputs`. 
    These files were used for the data presented in the manuscript.

    - `-o` : Output directory

    - `-model` : COBRA-compliant SBML file (generic human GEM)
        - File name in the source: `Recon2M.1_Entrez_Gene.xml`

    - `-gene_transcript_information` : A list of gene IDs and their matching transcript IDs
        - File name in the source: `Ensembl88_GRCh38_all_transcript_information.txt`

        **File format**

            NCBI gene ID    Gene stable ID  Transcript stable ID    RefSeq mRNA ID  UCSC Stable ID
            2733    ENSG00000119392 ENST00000309971 NM_001003722    uc004bvj.4
            2733    ENSG00000119392 ENST00000372770 NM_001499       uc004bvi.4
            5690    ENSG00000126067 ENST00000373237 NM_002794       uc001bzf.4
            5690    ENSG00000126067 ENST00000373237 NM_001199779    uc001bzf.4
            5690    ENSG00000126067 ENST00000621781 NM_001199780    uc021olh.3

        **Download procedure**

        1. Go to [Ensembl BioMarts](http://www.ensembl.org/biomart/martview)
        2. Click *Dataset* on the left menu

            Select *Ensembl Genes 89* in the drop-down menu *CHOOSE DATABASE*

            Select *Human genes (GRCh38.p10)* in the drop-down menu *CHOOSE DATASET*

        3. Click *Filters* on the left menu

            Click *GENE:* in the main menu (center)

            Check *Gene type* and select *protein_coding*

        4. Click *Attributes* on the left menu

            Check *Features* in the center

        5. Click both *GENE:* and *EXTERNAL:* in the main menu (center)

            *GENE:* -> *Ensembl* -> Uncheck *Gene stable ID* and *Transcript stable ID*

            Check following items in order:

            - *EXTERNAL:* -> *External References (max 3)* -> *NCBI gene ID*
            - *GENE:* -> *Ensembl* -> *Gene stable ID*
            - *GENE:* -> *Ensembl* -> *Transcript stable ID*
            - *EXTERNAL:* -> *External References (max 3)* -> *RefSeq mRNA ID*
            - *EXTERNAL:* -> *External References (max 3)* -> *UCSC Stable ID*

        6. Click the button *Results* on the top left
        7. Click the button *Go* in the top center

    *Implementation*
    **Note**: Running this script takes ~ 4 m

    .. code-block::
    
      $ python model_GPR_to_TPR_converter.py \
      -o ./results/GPR_to_TPR_results/ \
      -gene_transcript_information ./input_data/GPR_to_TPR_inputs/Ensembl88_GRCh38_all_transcript_information.txt \
      -model ./input_data/GPR_to_TPR_inputs/Recon2M.1_Entrez_Gene.xml


    *Feature: Update metabolite information*
    Input arguments and corresponding files
    Following working input files can be found in: `./input_data/metabolite_information_update_inputs`. 
    These files were used for the data presented in the manuscript.

    - `-o` : Output directory

    - `-model` : COBRA-compliant SBML file (generic human GEM)
        - File name in the source: `Recon2M.1_Entrez_Gene.xml`

    - `-mnx_xref` : Info on chemical identifiers from [MetaNetX](http://www.metanetx.org/)
        - File name in the source: `chem_xref.tsv`
        - Click [chem_xref.tsv](http://www.metanetx.org/cgi-bin/mnxget/mnxref/chem_xref.tsv) for downloading

    - `-mnx_prop` : Info on chemical structures from [MetaNetX](http://www.metanetx.org/)
        - File name in the source: `chem_prop.tsv`
        - Click [chem_prop.tsv](http://www.metanetx.org/cgi-bin/mnxget/mnxref/chem_prop.tsv) for downloading

    - `-bigg` : Info on BiGG metabolites from [BiGG Models](http://bigg.ucsd.edu/)
        - File name in the source: `bigg_models_metabolites.txt`
        - Click [bigg_models_metabolites.txt](http://bigg.ucsd.edu/static/namespace/bigg_models_metabolites.txt) for downloading

    - `-chebi` : Info on ChEBI and InChI from [ChEBI](https://www.ebi.ac.uk/chebi/init.do)
        - File name in the source: `chebiId_inchi.tsv`
        - Click [chebiId_inchi.tsv](ftp://ftp.ebi.ac.uk/pub/databases/chebi/Flat_file_tab_delimited/chebiId_inchi.tsv) for downloading

    *Implementation*
    **Note**: Running this script takes ~ 5 s

    .. code-block::
    
      $ python model_update_metabolite_information.py \
      -o ./results/metabolite_information_update_results/ \
      -model ./input_data/metabolite_information_update_inputs/Recon2M.1_Entrez_Gene.xml \
      -mnx_xref ./input_data/metabolite_information_update_inputs/chem_xref.tsv \
      -mnx_prop ./input_data/metabolite_information_update_inputs/chem_prop.tsv \
      -bigg ./input_data/metabolite_information_update_inputs/bigg_models_metabolites.txt \
      -chebi ./input_data/metabolite_information_update_inputs/chebiId_inchi.tsv


    *Feature: Calculate model statistics*
    Input arguments and corresponding files
    Following working input files can be found in: `./input_data/model_function_inputs`. 
    These files were used for the data presented in the manuscript.

    - `-o` : Output directory

    - `-model` : COBRA-compliant SBML file (generic human GEM)
        - File name in the source: `Recon2M.1_Entrez_Gene.xml`

    - `-medium` : A representative medium (RPMI-1640 medium)
        - File name in the source: `RPMI1640_medium.txt`

        **File format**
        
        .. code-block::

            EX_gly_LPAREN_e_RPAREN_	-0.05	1000
            EX_arg_L_LPAREN_e_RPAREN_	-0.05	1000
            EX_asn_L_LPAREN_e_RPAREN_	-0.05	1000
            EX_asp_L_LPAREN_e_RPAREN_	-0.05	1000
            EX_cys_L_LPAREN_e_RPAREN_	-0.05	1000
            EX_glu_L_LPAREN_e_RPAREN_	-0.05	1000
            EX_his_L_LPAREN_e_RPAREN_	-0.05	1000

    *Implementation*
    **Note**: Running this script takes ~ 47 m

    .. code-block::

      $ python model_metabolic_model_statistics.py \
      -o ./results/model_statistics_results/ \
      -medium ./input_data/model_function_inputs/RPMI1640_medium.txt \
      -model ./input_data/model_function_inputs/Recon2M.1_Entrez_Gene.xml


    *Feature: Evaluate functionality of metabolic model*
    Input arguments and corresponding files
    Following working input files can be found in: `./input_data/model_function_inputs`. 
    These files were used for the data presented in the manuscript.

    - `-o` : Output directory

    - `-model` : COBRA-compliant SBML file (generic human GEM)
        - File name in the source: `Recon2M.1_Entrez_Gene.xml`

    - `-medium` : A representative medium (RPMI-1640 medium)
        - File name in the source: `RPMI1640_medium.txt`

    - `-defined_medium` : A defined minimal medium
        - File name in the source: `Defined_medium.txt`

    - `-es_genes` : A list of essential genes
        - File name in the source: `Essential_genes_from_wang_et_al.txt`

    - `-ne_genes` : A list of non-essential genes
        - File name in the source: `Non_essential_genes_from_wang_et_al.txt`

    - `-c_source` : A list of carbon sources
        - File name in the source: `atp_carbon_source.txt`

    - `-biomass` : Reaction ID for biomass generation equation

    - `-oxygen` : Reaction ID for oxygen uptake

    - `-atp` : Reaction ID for ATP production

    *Implementation*
    **Note**: Directly insert reaction IDs in terminal for `-biomass`, `-oxygen` and `-atp`

    **Note**: Running this script takes ~ 7 m

    .. code-block::
    
      $ python model_metabolic_function.py \
      -o ./results/model_function_results/ \
      -model ./input_data/model_function_inputs/Recon2M.1_Entrez_Gene.xml \
      -medium ./input_data/model_function_inputs/RPMI1640_medium.txt \
      -defined_medium ./input_data/model_function_inputs/Defined_medium.txt \
      -es_genes ./input_data/model_function_inputs/Essential_genes_from_wang_et_al.txt \
      -ne_genes ./input_data/model_function_inputs/Non_essential_genes_from_wang_et_al.txt \
      -c_source ./input_data/model_function_inputs/atp_carbon_source.txt \
      -biomass biomass_reaction \
      -oxygen EX_o2_LPAREN_eRPAREN \
      -atp DM_atp_c_
    

    *Feature: Reconstruct personal GEMs using [tINIT](http://msb.embopress.org/content/10/3/721.long)*
    Input arguments and corresponding files
    Following working input files can be found in: `./input_data/tINIT_inputs`. 
    These files were used for the data presented in the manuscript.

    - `-o` : Output directory

    - `-model` : COBRA-compliant SBML file (generic human GEM)
        - File name in the source: `Recon2M.1_Entrez_Gene.xml`

    - `-medium` : A representative medium (RPMI-1640 medium)
        - File name in the source: `RPMI1640_medium.txt`

    - `-task` : A list of metabolic tasks
        - File name in the source: `MetabolicTasks.csv`

    - `-present_reaction` : A list of reactions that should be present in model
        - File name in the source: `essential_reactions.txt`

    - `-present_metabolite` : A list of metabolites that should be present in model
        - File name in the source: `essential_metabolites.txt`

    - `-i` : Omics data
        - File name in the source: `BLCA_T_TTL.csv`

    - `-biomass` : Reaction ID for biomass generation equation

    *Implementation*
    **Note**: Directly insert a reaction ID in terminal for `-biomass`

    **Note**: Running this script for a Recon model takes ~ 8 m

    .. code-block::
    
      $ python personal_GEM_tINIT.py \
      -o ./results/tINIT_results/ \
      -medium ./input_data/tINIT_inputs/RPMI1640_medium.txt \
      -model ./input_data/tINIT_inputs/Recon2M.1_Entrez_Gene.xml \
      -biomass biomass_reaction \
      -task ./input_data/tINIT_inputs/MetabolicTasks.csv \
      -present_reaction ./input_data/tINIT_inputs/essential_reactions.txt \
      -present_metabolite ./input_data/tINIT_inputs/essential_metabolites.txt \
      -i ./input_data/tINIT_inputs/BLCA_N_TTL.csv
    

    *Feature: Predict flux using transcript-level RNA-Seq data and GeTPRA*
    Input arguments and corresponding files
    Following working input files can be found in: `./input_data/Flux_prediction`. 
    These files were used for the data presented in the manuscript.

    - `-o` : Output directory

    - `-g_model` : COBRA-compliant SBML file (generic human GEM)
        - File name in the source: `Recon2M.2_BiGG_UCSC_Transcript.xml`

    - `-c_model` : COBRA-compliant SBML file (context-specific human GEM)
        - File name in the source: `LIHC_TCGA-BC-A10Q.xml`

    - `-getpra` : GeTPRA file 
        - File name in the source: `GeTPRA.txt`

    - `-use_getpra` : Option for flux prediction using transcript-level data
        - Insert`yes` for flux prediction using transcript-level data
        - Insert`no` for flux prediction using gene-level data

    *Implementation*
    **Note**: Running this script takes ~ 7 m

    .. code-block::
    
      $ python flux_prediction.py \
      -o ./results/Flux_prediction/ \
      -i ./input_data/Flux_prediction/LIHC_TCGA-BC-A10Q.csv \
      -getpra ./input_data/Flux_prediction/GeTPRA.txt \
      -g_model ./input_data/Flux_prediction/Recon2M.2_BiGG_UCSC_Transcript.xml \
      -c_model ./input_data/Flux_prediction/LIHC_TCGA-BC-A10Q.xml \
      -use_getpra yes
    

    **Publication**
    Jae Yong Ryu 1, Hyun Uk Kim 1 & Sang Yup Lee. Framework and resource for more than 11,000 gene-transcript-protein-reaction associations in human metabolism., Proc. Natl. Acad. Sci. U.S.A., 2017, http://www.pnas.org/content/early/2017/10/23/1713050114

    
    ------- 
