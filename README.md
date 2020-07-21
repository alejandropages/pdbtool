# PDBTOOL

## Table of contents
* [PDBTOOL installation on MacOS](#pdbtool-installation-on-macos)
* [Disclaimer](#disclaimer)
* [Commands](#commands)

# PLEASE REPORT ISSUES
send me and email at apages2@unl.edu with pdbtool in the subject line

# PDBTOOL installation on macOS

Steps to set up your environment for running older versions of python and python packages without messing up your current installations:

1. Install homebrew if you don’t already have it installed.
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
Source: 
	[homebrew](https://brew.sh/)

2. Using homebrew install pyenv and the pyenv-virtualenv plugin
```
$ brew update
$ brew install pyenv pyenv-virtualenv
```

Source: 
	[pyenv](https://github.com/pyenv/pyenv)
	[pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)

3. Add two lines to the end of your shell’s profile. 

```
eval "$(pyenv init - )"
eval "$(pyenv virtualenv-init -)"
```

Notes: Mac used bash by default until MacOS Catalina where it has started using zsh.
	To figure out what your default shell is, use this command: `$ echo $SHELL`
	If your shell is ‘bash’ then use `$ ls -a ~` to find either `~/.profile` or `~/.bash_profile`.
	If you are using zsh, your profile file is `~/.zprofile`. 
	Whatever be the case, append the above two lines.

   Activate your new profile:
```	
$ source ~/<profile-filename>
```
4. Use pyenv to install the newest version of python 3.5 (3.5.9).
	Note: this will not affect your default python installation. This makes the version of python specified available 
	to you when you want to use it.
```
$ pyenv install 3.5.9
```
5. Navigate to the directory where you would like the pdbtool repository.
```	
$ cd <your-installation-directory>
$ git clone https://github.com/alejandropages/pdbtool.git
$ cd pdbtool
$ pyenv local 3.5.9
$ pyenv virtualenv pdbtool
```
Note: after you run this command, whenever you are in this directory, your python version will be python3.5,
	outside of this dir it will be the default system python.

6. Finish setting up pdbtool environment:

```
$ pyenv activate pdbtools
$ pip install --upgrade pip
$ pip install scipy==1.4.1 matplotlib==3.0.3 lsq
```
7. Append the path to the pdbtool source folder PYTHONPATH and set pdbtool environment variables.
```
$ export PYTHONPATH=<path-to-installation-directory>:$PYTHONPATH
$ export PDBDOWNLOAD=<path-to-pdb-downloads-directory>
$ export PDBMIRROR=<path-to-pdb-mirrow-directory>
```

Note: you'll want to include these in your shell's profile as well.

7. You can now use pdbtool from anywhere in your filesystem using the following command and appending the path to the script you would like to call seperated by dots.

e.g. to call the script sstin.py located at pdbtools/scripts/sstin.py

```
$ python -m pdbtool.scripts.sstin  # is your virtualenv activated?
````

to call the script pdbtools/bbcomp.py

```
$ python -m pdbtool.bbcomp
```

If you are inside the pdbtool directory, you cannot use the -m flag with the dot sperator. instead you will need to use the path to the file you want run.

```
# from within the pdbtool directory
$ python scripts/sstin.py
```

## Disclaimer:

This tutorial is a work in progress and as such not every script may be fully functional given the configuration described here. Some scripts needed to be updated to python3 from python2 and not every script, function, and class were tested.

# Commands:
### acmerge
```
➜ python -m pdbtool.scripts.acmerge -h                                              
usage: acmerge.py [-h] [--acletters ACLETTERS] inpath1 inpath2 outpath

 
    Merges two models as alternative conformations

positional arguments:
  inpath1               The input PDB file of the first model.
  inpath2               The input PDB file of the second model.
  outpath               The output PDB file.

optional arguments:
  -h, --help            show this help message and exit
  --acletters ACLETTERS
                        Letters used to designate multiple conformers.
```
### cellsqueeze
```
➜ python -m pdbtool.scripts.cellsqueeze -h   
usage: cellsqueeze.py [-h] [--mrc MRC] [--output-mrc OUTPUT_MRC]
                      [--cushion CUSHION]
                      inpath outpath

 
    Reduces the unit cell to match the protein shape.  Currently
    only works in P1, which is consistent with the script's primary
    purpose of removing extra space from cryoEM models.

positional arguments:
  inpath                Input PDB file.
  outpath               Output PDB file.

optional arguments:
  -h, --help            show this help message and exit
  --mrc MRC             Corresponding map file
  --output-mrc OUTPUT_MRC
                        Output map file
  --cushion CUSHION     Cushion size around the molecule.
```
### contacts
```
➜ python -m pdbtool.scripts.contacts -h   
usage: contacts.py [-h] [--Dmax DMAX] inpath

 
    Calculates all the close contacts.

positional arguments:
  inpath       The input PDB file.

optional arguments:
  -h, --help   show this help message and exit
  --Dmax DMAX  Maximum contact distance, defaults to 4.0A.
```
### cylinder
```
➜ python -m pdbtool.scripts.cylinder -h
usage: cylinder.py [-h] [--Dmin DMIN] [--ranges RANGES] inpath

 
    Calculates a cylinder to cover the protein.

positional arguments:
  inpath           The input PDB file.

optional arguments:
  -h, --help       show this help message and exit
  --Dmin DMIN      Size of cushion, defaults to 3.2A.
  --ranges RANGES  Residue range selection, chain, start, end. Comma-
                   separated, no spaces. Separate chains with forward slash.
                   Example: A,50-55,72-80/B,50-55 will select residues 50 to
                   55 and 72 to 80 in chain A, and also residues 50 to 55 in
                   chain B.
```
### datcluster
```
➜ python -m pdbtool.scripts.datcluster -h 
usage: datcluster.py [-h] [--sqlpath SQLPATH] [--symmetric] [--d1 D1]
                     [--d2 D2] [--a1 A1] [--a2 A2] [--t1 T1] [--t2 T2]
                     [--torshift TORSHIFT] [--kmeans KMEANS] [--weighted]
                     [--title TITLE] [--hbtype HBTYPE]

DATCLUSTER reads a hb database that contains distance/angle/torsion columns
and performs cluster analysis.

optional arguments:
  -h, --help            show this help message and exit
  --sqlpath SQLPATH     HB database file. Defaults to hbpisa.sqlite.
  --symmetric           Bonds are chemically symmetric so both directions
                        should be included.
  --d1 D1               Distance left boundary.
  --d2 D2               Distance right boundary.
  --a1 A1               Angle left boundary.
  --a2 A2               Angle right boundary.
  --t1 T1               Torsion left boundary.
  --t2 T2               Torsion right boundary.
  --torshift TORSHIFT   Shift the torsion angle domain
  --kmeans KMEANS       Number of clusters to use to perform K-means
                        clustering analysis.
  --weighted            Weight distribution by redundancies (input file should
                        be processed by SEQFILTER
  --title TITLE         Title of the figure.
  --hbtype HBTYPE, -b HBTYPE
                        Hydrogen bond type. If provided, corresponding DAT
                        kernel parameters will be used as starting cluster
                        centroids.
```
### datcontour
```
➜ python -m pdbtool.scripts.datcontur -h 
usage: datcontur.py [-h] [--sqlpath SQLPATH] [--symmetric] [--dstep DSTEP]
                    [--astep ASTEP] [--tstep TSTEP] [--d1 D1] [--d2 D2]
                    [--a1 A1] [--a2 A2] [--t1 T1] [--t2 T2]
                    [--torshift TORSHIFT] [--normden] [--dakernel]
                    [--weighted] [--title TITLE]

DATCONTUR reads a hb database that contains distance/angle/torsion columns
and presents the results as distributions.

optional arguments:
  -h, --help           show this help message and exit
  --sqlpath SQLPATH    HB database file. Defaults to hbpisa.sqlite.
  --symmetric          Bonds are chemically symmetric so both directions
                       should be included.
  --dstep DSTEP        Distance step size.
  --astep ASTEP        Angle step size.
  --tstep TSTEP        Torsion step size.
  --d1 D1              Distance left boundary.
  --d2 D2              Distance right boundary.
  --a1 A1              Angle left boundary.
  --a2 A2              Angle right boundary.
  --t1 T1              Torsion left boundary.
  --t2 T2              Torsion right boundary.
  --torshift TORSHIFT  Shift the torsion angle domain
  --normden            Use density option when building histograms.
  --dakernel           Calculate parameters of the DA kernel (slow).
  --weighted           Weight distribution by redundancies (input file should
                       be processed by SEQFILTER
  --title TITLE        Title of the figure.
```
### datrefine
```
➜ python -m pdbtool.scripts.datrefine -h
usage: datrefine.py [-h] [--sqlpath SQLPATH] [--torshift TORSHIFT]
                    [--pvalue PVALUE] [--hbtype HBTYPE] [--title TITLE]

DATREFINE reads a hb database that contains distance/angle/torsion 
columns and refines the DAT parameters.

optional arguments:
  -h, --help            show this help message and exit
  --sqlpath SQLPATH     HB database file. Defaults to hbpisa.sqlite.
  --torshift TORSHIFT   Shift the torsion angle domain
  --pvalue PVALUE       p-value cutoff to use for refinement.
  --hbtype HBTYPE, -b HBTYPE
                        Hydrogen bond type.
  --title TITLE         Title of the figure.
```
### hbfilter
```
➜ python -m pdbtool.scripts.hbfilter -h 
usage: hbfilter.py [-h] [--sqlpath SQLPATH] [--sqlpisa SQLPISA]
                   [--sqlout SQLOUT] [--in-place] [--mmsize MMSIZE]
                   [--res2 RES2] [--same-residue] [--not-same-residue]
                   [--do-checks] [--pvalue PVALUE] [--hbtype HBTYPE]
                   [--symmetric]

 
    HBFILTER selects a subset of contacts from a database

optional arguments:
  -h, --help            show this help message and exit
  --sqlpath SQLPATH     HB database file. Defaults to hbpisa.sqlite.
  --sqlpisa SQLPISA     PISA database file. Defaults to pisa.sqlite.
  --sqlout SQLOUT       Output HB database file. Defaults to
                        hbfiltered.sqlite.
  --in-place            Modify the database file in place, use with caution
  --mmsize MMSIZE       Oligomerization number. Could be a comma separated
                        list.
  --res2 RES2           Filter by second residue type
  --same-residue        Filter for identical residues forming a bond
  --not-same-residue    Filter for identical residues forming a bond
  --do-checks           Check the PISA database integrity.
  --pvalue PVALUE       pvalue cutoff. This requires that the type of hydrogen
                        is specified.
  --hbtype HBTYPE, -b HBTYPE
                        Hydrogen bond type.
  --symmetric           Bonds are chemically symmetric so both directions
                        should be included.
```
### hblist
```
➜ python -m pdbtool.scripts.hblist -h  
usage: hblist.py [-h] [--ohmax OHMAX] [--nomax NOMAX] [--dhamin DHAMIN]
                 [--pcutoff PCUTOFF] [-b BONDTYPE] [--ranges RANGES]
                 model

HBLIST lists the hydrogen bonds in a structure.

positional arguments:
  model                 Model PDB file.

optional arguments:
  -h, --help            show this help message and exit
  --ohmax OHMAX         O...H distance cutoff for main chain hydrogen bonds
  --nomax NOMAX         N...O distance cutoff for main chain hydrogen bonds
  --dhamin DHAMIN       Minimum donor...hydrogen...acceptor angle in hydrogen
                        bonds
  --pcutoff PCUTOFF     p-value cutoff for including hydrogen bonds
  -b BONDTYPE, --bondtype BONDTYPE
                        Comma-separated list of bond types you wish to show.
                        Defaults to "all"
  --ranges RANGES       Residue range selection, chain, start, end. Comma-
                        separated, no spaces. Separate chains with forward
                        slash. Example: A,50-55,72-80/B,50-55 will select
                        residues 50 to 55 and 72 to 80 in chain A, and also
                        residues 50 to 55 in chain B.
```
### hbpisa
```
➜ python -m pdbtool.scripts.hbpisa -h
usage: hbpisa.py [-h] [--sqlpath SQLPATH] [--sqlpisa SQLPISA]
                 [--hbtype HBTYPE] [--do-checks]

 
    Builds HB database for dataset derived from PISA

optional arguments:
  -h, --help            show this help message and exit
  --sqlpath SQLPATH     HB database file. Defaults to hbpisa.sqlite.
  --sqlpisa SQLPISA     PISA database file. Defaults to pisa.sqlite.
  --hbtype HBTYPE, -b HBTYPE
                        Hydrogen bond type (mandatory).
  --do-checks           Check the PISA database integrity.
```
### normocc
```
➜ python -m pdbtool.scripts.normocc -h
usage: normocc.py [-h] [-f] inpath [outpath]

 
    Normalize occupancies for alternate conformers that exceed total
    occupancy of 1.    

positional arguments:
  inpath                The input PDB file.
  outpath               The output PDB file. If not specified, input model is
                        simply checked for occupancy issues.

optional arguments:
  -h, --help            show this help message and exit
  -f, --force-overwrite
                        Overwrite existing files.
```
### pdbplots
```
➜ python -m pdbtool.scripts.pdbplots -h
usage: pdbplots.py [-h] [-p] inpath

 
    PDB file analysis plots.  Actions to perform on the input file are 
    defined by -p option. Allowed actions are:
    
    bvalues             Plots average per-residue Bfactors, 
                        including all atoms, backbone and side chain 
                        columns.

positional arguments:
  inpath        The input PDB file.

optional arguments:
  -h, --help    show this help message and exit
  -p , --plot   Plotting options.
```
### pdbtricks
```
➜ python -m pdbtool.scripts.pdbtricks -h
usage: pdbtricks.py [-h] [-a] [-p] [--resid RESID] [--bvalue-print]
                    [--chids CHIDS] [--extract-chains] [--rjust-resid]
                    [--ranges RANGES] [--window-size WINDOW_SIZE]
                    [--rcutoff RCUTOFF]
                    inpath [outpath]

PDB file manipulations.  Actions to perform on the input file are 
defined by -a option. Allowed actions are:

    extract-chains      Extract only chains specified by --chids option
    extract-ranges      Extracts atoms that belong to the list of 
                        residue ranges
    rename-chains       Rename chains using the pattern defined by
                        --chids option.  For example, "AB,CD" will
                        rename A to B and C to D.
    rjust-resid         Makes sure residue names are right-justified
    tinertia-ranges     Outputs tensor of inertia information for 
                        specified ranges
    tinertia-slider     Outputs tensor of inertia information along the
                        sequence
    center-orient       Centers the molecule at the center of mass and
                        orients it along inertia axes
    set-b-per-chain     Sets B-factors to chain average

Program will also print various information extracted from the input 
PDB file. Output is defined by -p option.  Currently supported choices 
are:

    bvalue              Prints the list of average per-residue Bfactors, 
                        including all atoms, backbone and side chain 
                        columns.
    chains              Prints the list of chains with number of atoms 
                        in each and average B factor.
    phipsi              Prints the list of backbone torsions.
    bcontrast           Calculate the average B-factor of a particular
                        residue range (must be specified with --ranges
                        option) and that of its immediate environment,
                        defined as all the non-water atoms within 
                        cutoff distance
    hbonds              Prints the list of hydrogen bonds
    range_bs            Prints the average B-factor for a selected range

--------------------------------------------------------------------------------

positional arguments:
  inpath                The input PDB file.
  outpath               The output PDB file.

optional arguments:
  -h, --help            show this help message and exit
  -a , --action         Action to perform
  -p , --outprint       Information to print out.
  --resid RESID         Residue id parameter for various commands.
  --bvalue-print        Print per-residue B-balues.
  --chids CHIDS         Chain IDs for various selections.
  --extract-chains      Extract specified chains.
  --rjust-resid         Make sure resids are right-justified.
  --ranges RANGES       Residue range selection, chain, start, end. Comma-
                        separated, no spaces. Separate chains with forward
                        slash. Example: A,50-55,72-80/B,50-55 will select
                        residues 50 to 55 and 72 to 80 in chain A, and also
                        residues 50 to 55 in chain B.
  --window-size WINDOW_SIZE
                        Sliding sequence window size.
  --rcutoff RCUTOFF     Distance cutoff, defaults to 4A
```
### pisaset
```
➜ python -m pdbtool.scripts.pisaset -h  
usage: pisaset.py [-h] [--inpath INPATH] [--sqlpath SQLPATH] [--no-checks]

 
    Updates PISA datasets.

optional arguments:
  -h, --help            show this help message and exit
  --inpath INPATH, -i INPATH
                        The input files listing PISA entries.
  --sqlpath SQLPATH     Database file
  --no-checks           Do not check the database integrity.
```
### sstin
```
➜ python -m pdbtool.scripts.sstin -h
usage: sstin.py [-h] [-p PDB] [-l LIST_FILE] [-d SQLITE_FILE]

SSTIN runs the survey of inertia ellipsoids of secondary structure
        elements.

optional arguments:
  -h, --help            show this help message and exit
  -p PDB, ---pdb PDB    Input pdb file. This parameter is either a PDB (not
                        case-sensitive) code or path to a PDB file.
  -l LIST_FILE, --list-file LIST_FILE
                        List of PDB codes to process. Results are stored in
                        SQLite database.
  -d SQLITE_FILE, --sqlite-file SQLITE_FILE
                        SQLite database to store/retrieve processed results.
```
### tinalign
```
➜ python -m pdbtool.scripts.tinalign -h
usage: tinalign.py [-h] [--chids CHIDS] [--spin {x,y,z}] [--swap]
                   fixmodel movmodel outmodel

TINALIGN aligns two structures by aligning their tensors of inertia.

positional arguments:
  fixmodel        Fixed model PDB file.
  movmodel        Moving model PDB file.
  outmodel        Output model PDB file.

optional arguments:
  -h, --help      show this help message and exit
  --chids CHIDS   Selection of chids
  --spin {x,y,z}  Spin around selected axis.
  --swap          Swap similar axis
```
### bbcomp
```
➜ python -m pdbtool.bbcomp -h       
usage: bbcomp.py [-h] [-l] [--delta-phi DELTA_PHI] [--delta-psi DELTA_PSI]
                 [--delta-omega DELTA_OMEGA] [--bbout BBOUT]
                 model1 model2

BBCOMP runs the comparison of backbone angles (phi/psi/omega) between
    two conformations.

positional arguments:
  model1                First model PDB file.
  model2                Second model PDB file.

optional arguments:
  -h, --help            show this help message and exit
  -l, --list            List the residues over the cutoff values
  --delta-phi DELTA_PHI
                        Phi angle change cutoff.
  --delta-psi DELTA_PSI
                        Psi angle change cutoff.
  --delta-omega DELTA_OMEGA
                        Omega angle change cutoff.
  --bbout BBOUT         Path to the output PDB file showing backbone
                        differences as B-factors.
```
### hbcomp
```
➜ python -m pdbtool.hbcomp -h
usage: hbcomp.py [-h] [--ohmax OHMAX] [--nomax NOMAX] [--dhamin DHAMIN]
                 [--pcutoff PCUTOFF] [-b BONDTYPE] [--bbout BBOUT]
                 model1 model2

HBNCOMP analyzes changes in hydrogen bonding patterns.

positional arguments:
  model1                First model PDB file.
  model2                Second model PDB file.

optional arguments:
  -h, --help            show this help message and exit
  --ohmax OHMAX         O...H distance cutoff for main chain hydrogen bonds
  --nomax NOMAX         N...O distance cutoff for main chain hydrogen bonds
  --dhamin DHAMIN       Minimum donor...hydrogen...acceptor angle in hydrogen
                        bonds
  --pcutoff PCUTOFF     p-value cutoff for including hydrogen bonds
  -b BONDTYPE, --bondtype BONDTYPE
                        Comma-separated list of bond types you wish to show.
                        Defaults to "all"
  --bbout BBOUT         Path to the output PDB file showing backbone
                        differences as B-factors.
```
### tincomp
```
➜ python -m pdbtool.tincomp -h
usage: tincomp.py [-h] [--simple-output] model1 model2 segfile

TINCOMP analyzes structural changes using tensor of inertia changes
throughout protein structure.

positional arguments:
  model1           First model PDB file.
  model2           Second model PDB file.
  segfile          Input file with structural segments.

optional arguments:
  -h, --help       show this help message and exit
  --simple-output  Formatted output for easy parsing.
```
