GeTPRA
======

.. automodule:: GeTPRA 
    :members: 
    :undoc-members: 
    :show-inheritance: 
    
    **The GeTPRA framework**
    
    This project is to develop a framework that systematically predicts Gene-Transcript-Protein-Reaction Associations (GeTPRA) in human metabolims and updates a human genome-scale metabolic model (GEM) accordingly. This source code implements the GeTPRA framework.
    
    ------
    
    **Features**
    This source code executes following steps in the GeTPRA framework in order:

    - Get reactions from biochemical database using EC number
    - Standardize metabolite information
    - Compartmentalize metabolic reactions
    - Generate GeTPRA
    - Check 'Exist in Recon 2M.1'
    - Check 'Blocked reaction'
    - Check 'Experimental evidence available'

    See below for the implementation of EFICAz and Wolf PSort (optional features)

    **Installation**
    **Procedure**
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

    **Input files for the GeTPRA framework**
    Following working input files can be found in: `getpra/input_data/getpra_inputs/`. These files were used for the data presented in the manuscript.

    **Gene-transcript ID annotation file format**
    - Download gene-transcript ID annotation file from [Ensembl BioMarts](http://www.ensembl.org/biomart/martview)
    
    .. code-block::

            NCBI gene ID	Gene stable ID	Transcript stable ID	RefSeq mRNA ID	UCSC Stable ID
            2733	ENSG00000119392	ENST00000309971	NM_001003722	uc004bvj.4
            2733	ENSG00000119392	ENST00000372770	NM_001499	uc004bvi.4
            5690	ENSG00000126067	ENST00000373237	NM_002794	uc001bzf.4
            5690	ENSG00000126067	ENST00000373237	NM_001199779	uc001bzf.4
            5690	ENSG00000126067	ENST00000621781	NM_001199780	uc021olh.3

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

    - File names in the source:
        - `Ensembl_GRCh38_EnsemblDB_v84.txt`
        - `Ensembl_GRCh38_EnsemblDB_v85.txt`
        - `Ensembl_GRCh38_EnsemblDB_v86.txt`
        - `Ensembl_GRCh38_EnsemblDB_v87.txt`
        - `Ensembl_GRCh38_EnsemblDB_v88.txt`

    *`chem_xref.tsv` from `MetaNetX`*
    - Download [chem_xref.tsv](http://www.metanetx.org/cgi-bin/mnxget/mnxref/chem_xref.tsv) from [MetaNetX](http://www.metanetx.org/)
    - File name in the source: `chem_xref.tsv`

    *`gene2ensembl.gz` from `NCBI FTP`*
    - Download [gene2ensembl.gz](ftp://ftp.ncbi.nlm.nih.gov/gene/DATA/gene2ensembl.gz) from [NCBI FTP](ftp://ftp.ncbi.nlm.nih.gov/)
    - File name in the source: `gene2ensembl.gz`

    *`appris_data.principal.txt` from `APPRIS`*
    - Download [appris_data.principal.txt](http://apprisws.bioinfo.cnio.es/pub/current_release/datafiles/homo_sapiens/GRCh38/appris_data.principal.txt) for annotation of principal isoform from [APPRIS](http://appris.bioinfo.cnio.es/)
    - File name in the source: `appris_data.principal.txt`

    *`subcellular_location.csv` from `The Human Protein Atlas`*
    - Download [subcellular_location.csv](http://www.proteinatlas.org/download/subcellular_location.csv.zip) with subcellular localization information from [The Human Protein Atlas](http://www.proteinatlas.org/)
    - File name in the source: `subcellular_location.csv`

    *Recon model*
    - Prepare a human genome-scale metabolic model with consistent TPR associations and that shows biologically reasonable simulation performance
    - File name in the source: `Recon2M.1_Entrez_Gene.xml`

    *EFICAz output file as input file*
    - File name in the source: `20170110_EFICAz_result.txt`.
    - EFICAz can be run with a different set of peptide sequences (see below).

    *WoLF PSort output file as input file*
    - File name in the source: `20170110_WoLFPSort_result.txt`
    - WoLF PSort can be run with a different set of peptide sequences (see below).

    *BRENDA data*
    - Set user email address and password before implementing the GeTPRA framework. The framework programmatically fetches BRENDA data from BRENDA through its API.

    ##Implementation
    **Note**: All the arguments shown below should be provided when implementing the framework

    **Note**: Make sure to provide own information for `-brenda_email` and `-brenda_pw`

    **Note**: Implementation of this source code takes long (~ 8 h)
    
    .. code-block::

        $ python run_GeTPRA_framework.py \
            -output_dir ./getpra_results/ \
            -ec ./input_data/getpra_inputs/20170110_EFICAz_result.txt \
            -sl ./input_data/getpra_inputs/20170110_WoLFPSort_result.txt \
            -brenda_email user_email_address \
            -brenda_pw user_password \
            -mnx_xref ./input_data/getpra_inputs/chem_xref.tsv \
            -ensembl ./input_data/getpra_inputs/Ensembl_GRCh38_EnsemblDB_v88.txt \
            -appris ./input_data/getpra_inputs/appris_data.principal.txt \
            -model ./input_data/getpra_inputs/Recon2M.1_Entrez_Gene.xml \
            -hpa ./input_data/getpra_inputs/subcellular_location.csv \
            -ncbi_id_information ./input_data/getpra_inputs/gene2ensembl.gz

    ##Output files from the GeTPRA framework
    - Raw output files from the GeTPRA framework, which were used for the publication, are available in: `getpra/getpra_results_publication_version/`
    - New output files upon implementation of the framework are generated in: `getpra/getpra_results/`. This folder is automatically created.

    *Implementation of EFICAz and Wolf PSort (optional)*
    Output files of EFICAz and Wolf PSort serve as input files for the GeTPRA framework.

    *Peptide sequences of metabolic genes as inputs for EFICAz and Wolf PSort*
    - Get peptide sequences of metabolic genes by implementing `./input_data/get_peptide_sequences.py` 
    
    .. code-block::

            $ python ./input_data/get_peptide_sequences.py \
            -output_dir ./input_data/getpra_inputs/ \
            -model ./input_data/getpra_inputs/Recon2M.1_Entrez_Gene.xml \
            -ensembl ./input_data/getpra_inputs/Ensembl_GRCh38_EnsemblDB_v88.txt

    - File name in the source: `Ensembl_peptide_seq_metabolic_genes.fa`

            >ENSP00000452494|ENST00000448914
            TGGY
            >ENSP00000488240|ENST00000631435
            GTGG
            >ENSP00000487941|ENST00000632684
            GTGG
            >ENSP00000451515|ENST00000434970
            PSY
            >ENSP00000451042|ENST00000415118
            EI

    *Installation*
    1. Download [EFICAz2.5](http://cssb.biology.gatech.edu/skolnick/webservice/EFICAz2/index.html)

    2. Set environment variable for EFICAz2.5
    
      .. code-block::

            $ export EFICAz25_PATH="[insert-destination-directory]/EFICAz2.5.1/"
            $ export PATH="${PATH}:${EFICAz25_PATH}"

    3. Download [WoLF PSort](https://github.com/fmaguire/WoLFPSort)

    4. Set environment variable for WoLF PSort
    
      .. code-block::

            $ export WoLFPSort_PATH="[insert-destination-directory]/WoLFPSort/"
            $ export PATH="${PATH}:${WoLFPSort_PATH}"

    *Implementation of EFICAz and Wolf PSort*
    - Predict EC numbers using [EFICAz2.5](http://cssb.biology.gatech.edu/skolnick/webservice/EFICAz2/index.html)
    
      .. code-block::

            $ python $EFICAz25_PATH/eficaz2.5 Ensembl_peptide_seq_metabolic_genes.fa

    - Predict subcellular localization using [WoLF PSort](https://github.com/fmaguire/WoLFPSort)
    
      .. code-block::

            $ WoLFPSort_PATH/bin/runWolfPsortSummary animal < Ensembl_peptide_seq_metabolic_genes.fa

    - Output files from the above two implementations using peptide sequences of entire human genes are available in:
        - EFICAz: `getpra/input_data/20170110_EFICAz_result_using_all_human_genes.txt`
        - Wolf PSort: `getpra/input_data/20170110_WoLFPSort_result_using_all_human_genes.txt`

    *Extract metabolic genes from EFICAz and Wolf PSort output data obtained with entire human genes*
    - Extract metabolic genes from the EFICAz and Wolf PSort output data 
      
      .. code-block::
      
            $ python ./input_data/get_EFICAz_WolfPSort_results.py \
            -output_dir ./input_data/getpra_inputs/ \
            -model ./input_data/getpra_inputs/Recon2M.1_Entrez_Gene.xml \
            -ec ./input_data/getpra_inputs/20170110_EFICAz_result_using_all_human_genes.txt \
            -sl ./input_data/getpra_inputs/20170110_WoLFPSort_result_using_all_human_genes.txt \
            -ensembl ./input_data/getpra_inputs/Ensembl_GRCh38_EnsemblDB_v88.txt

    - Resulting output files in the source:
        - EFICAz: `getpra/input_data/Trimmed_EFICAz_result.txt`
        - Wolf PSort: `getpra/input_data/Trimmed_WoLFPSort_result.txt`

    **Publication**
    Jae Yong Ryu 1, Hyun Uk Kim 1 & Sang Yup Lee. Framework and resource for more than 11,000 gene-transcript-protein-reaction associations in human metabolism., *Proc. Natl. Acad. Sci. U.S.A.*, 2017, http://www.pnas.org/content/early/2017/10/23/1713050114

    
    
    ------- 
