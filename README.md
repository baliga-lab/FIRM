# FRAMEWORK FOR INFERENCE OF REGULATION BY MIRNAS (FIRM)

By integrating the three best performing algorithms to infer miRNA mediate regulation from co-expression signatures we have constructed a generalizable Framework for Inference of Regulation by miRNAs (FIRM). FIRM is available as a Python script capable of identifying putative miRNA mediated regulation from transcriptome co-expression signatures.

## FIRM DEPENDENCIES
FIRM was developed in Linux, but by ensuring the following dependencies are met could be modified to work in Windows. In order to function properly this script requires:

Python >= 2.6 or Python 3.x
Weeder: we use a patched variant of version 1.4.2 that is more flexible in where input paths are expected https://github.com/baliga-lab/weeder_patched
and fixes some memory leak issues

## FIRM FILES
Here is a brief description of the files and folders in the FIRM directory:
    prompt> ls FIRM

    FIRM.py
    TargetPredictionDatabases
    common
    exp
    miRvestigator.py
    pssm.py
    FreqFiles
    
    * FIRM.py - is the script to run the analysis.
    * TargetPredictionDatabases - directory containing the target prediction databases PITA and TargetScan.
    * common - directory containing general files needed to run the analysis.
    * exp - directory where you should put your co-expression signatures files that are to be analyzed. Each file in this directory is expected to use the naming convention of having three names separated by underscores (_) and the extension '.sgn', e.g. "AD_Lung_Beer.sgn". If you want to change this requirement please modify lines 450 and 451 of the "Firm.py" file. The files should look like this:
        Gene	Group
        NM_000014	32
        NM_000015	23
        ...
      This file is expected to have a header. It has two columns:
       1. RefSeq Transcript ID, alternatively you can use Entrez IDs
       2. Co-expression signature number
    * miRvestigator.py - python module of the miRvestiagtor analysis script.
    * pssm.py - python module to be a container for PSSM objects.
    * FreqFiles - the Weeder frequence files that should be placed in the appropriate FreqFile location as determined by the way Weeder was installed.

## INSTALLING AND RUNNING FIRM
If the dependencies above are met then to run FIRM will simply require the user to create the appropriate co-expression signature files and place them in the 'exp' directory. Then the analysis can be started by typing:

    prompt> python FIRM.py

## INTERPRETING RESULTS FROM FIRM
FIRM limits the Weeder-miRvestigator method to only those inferences of miRNA mediated regulation with a perfect 7- or 8-mer miRvestigator complementarity p-value (p-value = 6.1 x 10-5 or 1.5 x 10-5, respectively) to a miRNA seed in miRBase. Inferences of miRNA mediated regulation from the PITA and TargetScan enrichment of predicted miRNA target genes methods were filtered to include only those with Benjamini-Hochberg FDR = 0.001 and at least 10% of genes had a predicted miRNA binding site. After FIRM finished running it will produce a file 'combinedResults.csv' in the main FIRM directory. For the This file will contain a listing of all co-expression signatures that were predicted to be regualted by a miRNA. The column headings are:

    1. Dataset - the dataset where the co-expression signature was observed.
    2. signature - nubmer for the co-expression signature.
    3. miRvestigator.miRNA - miRvestigator predicted miRNA(s).
    4. miRvestigator.model - model that fit the miRNA to the PSSM found through Weeder, either 7mer or 8mer.
    5. miRvestigator.mature_seq_ids - the miRBase mature sequence IDs for the predicted miRNAs.
    6. PITA.miRNA - PITA predicted mirna(s)
    7. PITA.percent_targets - percent of co-expression cluster genes with predicted target sites in PITA.
    8. PITA.P_Value - p-value for the hypergeometric enrichment of miRNA(s).
    9. PITA.mature_seq_ids - the miRBase mature sequence IDs for the predicted miRNAs.
    10. TargetScan.miRNA - TargetScan predicted mirna(s)
    11. TargetScan.percent_targets - percent of co-expression cluster genes with predicted target sites in TargetScan.
    12. TargetScan.P_Value - p-value for the hypergeometric enrichment of miRNA(s).
    13. TargetScan.mature_seq_ids - the miRBase mature sequence IDs for the predicted miRNAs.
