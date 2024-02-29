# Création de l'environnement : 
```
conda create --name myenv python=3.9.15
conda activate myenv
```

# SCIP solver
Installation de scipoptsuite-6.0.1.tgz qui contient SCIP 6.0.1 / SoPlex version 4.0.1
https://www.scipopt.org/index.php#download

Une fois ceci fait : extracter seulement le fichier de scip et soplex dans le projet

Set-up a desired installation path for SCIP / SoPlex (e.g., `/opt/scip`):
```
export SCIPOPTDIR='/opt/scip'
```

## SoPlex

SoPlex 4.0.1 (free for academic uses)
```
cd soplex/
mkdir build
cmake -S . -B build -DCMAKE_INSTALL_PREFIX=$SCIPOPTDIR
make -C ./build -j 4
make -C ./build install
cd ..
```

## SCIP

SCIP 6.0.1 (free for academic uses)

https://scip.zib.de/download.php?fname=scip-6.0.1.tgz

```
cd scip-6.0.1/
```

Apply patch file in `learn2branch/scip_patch/`

```
patch -p1 < ../learn2branch/scip_patch/vanillafullstrong.patch ( Si cela ne fonctionne pas, essayez d'utiliser le chemin complet : patch -p1 < /home/loay/learn2branch/scip_patch/vanillafullstrong.patch ) 
```

```
mkdir build
cmake -S . -B build -DSOPLEX_DIR=$SCIPOPTDIR -DCMAKE_INSTALL_PREFIX=$SCIPOPTDIR
make -C ./build -j 4
make -C ./build install
cd ..
```

# Python dependencies

Recommended setup: conda + python 3

https://docs.conda.io/en/latest/miniconda.html

## Cython

Required to compile PySCIPOpt and PySVMRank
```
pip install Cython==3.0.0a11
```

## PySCIPOpt

SCIP's python interface (modified version)

```
pip install git+https://github.com/ds4dm/PySCIPOpt.git@ml-branching
```

## ExtraTrees
```
conda install scikit-learn=0.20.2  # ExtraTrees
```

## LambdaMART
```
pip install git+https://github.com/jma127/pyltr@78fa0ebfef67d6594b8415aa5c6136e30a5e3395  # LambdaMART
```

## SVMrank
```
git clone https://github.com/ds4dm/PySVMRank.git
cd PySVMRank
wget http://download.joachims.org/svm_rank/current/svm_rank.tar.gz  # get SVMrank original source code
// long verbosity;  # comment line 34 in src/c/svm_light/svm_hideo.c, it seems to fix the problem  
mkdir src/c
tar -xzf svm_rank.tar.gz -C src/c
pip install .
```

## Tensorflow
```
conda install tensorflow-gpu=1.12.0   # Version 1 de tenserflow non compatible 
```
