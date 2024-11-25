# Photo-Driven-Li-Extraction-Cages
This repository contains the scripts and xyz-files relevant for studying the Li-extraction ability of photo-active azobipyridine-based coordination cages..

## Software used
- [Scigress](https://www.fqs.pl/en/chemistry/products/scigress) v2.6
- [ORCA](https://www.faccts.de/orca/) v5.0.4
- [xtb](https://github.com/grimme-lab/xtb) v6.5.0
- [OPTIM](https://www-wales.ch.cam.ac.uk/OPTIM/)
- [GMIN](https://www-wales.ch.cam.ac.uk/GMIN/)
- SLURM scheduler (for job arrays)

For installing these tools, refer to their respective documentation or package managers suitable for your system.

## Repository Structure
- 1-Coordination/: Input-files and output files for section 7.1 of the Supporting Information
- 2-Npositions/: Input-files and output files for section 7.2 of the Supporting Information
- 3-PhenylOrientations/: Input-files and output files for section 7.2 of Supporting Information

## 1. Investigation of the Li-coordination environments
1) MM3 modelling was performeed in Scigress on four different potential geometries of the sandwich complex Li<sub>4</sub>L<sub>2</sub>. Each started out from a different coordination geometry.

2) Geometry optimizations at the GFN2-xTB level performed on the MM3-optimized from step 1 using the *OPTIM* interface to the *XTB* program. For each calculation, a charge of 5 was applied. The odata file shown below contains all the input parameters:

   odata:
   ```
    DEBUG

    BFGSMIN 1.0D-6
    CONVERGE 0.1 1.0D-6
    UPDATES 500 100 5 5
    MAXERISE 1.0D-5 0.02D0
    MAXBFGS 0.2 0.1
    BFGSSTEPS 10000

    RADIUS 2000.0
    DUMPDATA
    ENDNUMHESS

    XTB azo-coordinate.xyz ' --acc 0.01 --chrg 5 --gfn2 '
   ```

   The program is run by adding the following line to the *sbatch.optim* file,
   ```
   /path-to-OPTIM/OPTIM > output
   ```
   followed by running:
   
   >>sbatch sbatch.optim

   The results of the optimization can be found in the 1-Coordination folder as *1[a,b,c,d]_output* and *1[a,b,c,d]_GFN2_converged.xyz*.
   
## 2. Investigation of the relative pyrrole-nitrogen positions
1) Four diastereomers of 1a (1a_alpha, 1a_beta, 1a_gamma, 1a_delta) were created by changing the moving one nitrogen around the pyrrole-ring starting from 1a. These files can be found in the repository as *1a_[type]_GFN2/1a_[type]_input.xyz*, with type being alpha, beta gamma or delta.
   
2) Geometry optimizations at the GFN2-xTB level were performed on the input structures generated in step 1. Well-converged minima ...

3) Geometry optimizations at the r<sup>2</sup>SCAN-3c level were performed on the structures obtained in step 2. First, optimizations at the NormalSCF convergence criteria were performed, followed by optimizations at the TightSCF convergence criteria. Initially, no convergence could be reached via this method for 1a and 1a_gamma. In both cases, well-converged minima were obtained by choosing a different input structure by adjusting the converged structure of a different diastereomer. 

## 3. Investigation of relative orientations of the phenyl rings
