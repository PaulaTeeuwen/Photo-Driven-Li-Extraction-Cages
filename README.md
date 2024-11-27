# Photo-Driven-Li-Extraction-Cages
This repository contains the scripts and xyz-files relevant for studying the Li-extraction ability of photo-active azobipyridine-based coordination cages.

## Software used
- [Scigress](https://www.fqs.pl/en/chemistry/products/scigress) v2.6
- [ORCA](https://www.faccts.de/orca/) v5.0.4
- [xtb](https://github.com/grimme-lab/xtb) v6.5.0
- [OPTIM](https://www-wales.ch.cam.ac.uk/OPTIM/)
- [GMIN](https://www-wales.ch.cam.ac.uk/GMIN/)
- SLURM scheduler (for job arrays)

For installing these tools, refer to their respective documentation or package managers suitable for your system.

## Repository Structure
- 1-Coordination/: Input-files and output files for section 1.
- 2-NPositions/: Input-files and output files for section 2.
- 3-PhenylOrientations/: Input-files and output files for section 3.

## 1. Investigation of the Li-coordination environments
1) MM3 modelling was performed in Scigress on four different potential geometries of the sandwich complex Li<sub>4</sub>L<sub>2</sub>. The calculations started from guess-structures with different coordination environments around the Li-atoms. These different environments were created by explicitly drawing (or not drawing) coordination bonds between the Li and different combinations of nitrogen atoms. The MM3 optimized structures can be found in the repository as *1[a,b,c,d]_MM3.xyz* in the *1-Coordination* folder.

2) Geometry optimizations at the GFN2-xTB level performed on the MM3-optimized cages from step 1.1 using the OPTIM interface to the XTB program. For each calculation, a charge of 5 was applied. The odata file shown below contains all the input parameters used during the calculation:

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
   
   Well-converged minima were obtained (*1[a,b,c,d]_GFN2_converged.xyz*) and their energies were recorded for comparison (see *1[a,b,c,d]_output*)

3) Basin-hopping global optimizations were performed on each of the four structures to explore configuraiont space. Thes calculations were performed using the GMIN program via the xtb interface. Both the GFN-FF and GFN2-xTB levels of theory were used. GMIN calculations at the GFN-FF level were followed by relaxations at the GFN2-xTB level of all found minima via the OPTIM interface to xtb. These searches did not locate any physically relevant lower energy minima. Input and output files for these calculations are not added to this repository.
   
## 2. Investigation of the relative pyrrole-nitrogen positions
1) Four diastereomers of 1a (1a_alpha, 1a_beta, 1a_gamma, 1a_delta) were created by moving one nitrogen around the pyrrole-ring starting from 1a. The five structures can be found in the repository as *1a[_type]_GFN2/1a[_type]_input.xyz*, where *_type* is either nothing, *_alpha*, *_beta*, *_gamma* or *_delta*.
   
2) Geometry optimizations at the GFN2-xTB level were performed on the input structures generated in step 2.1 analogous to step 1.2. Well-converged minima were obtained (*1a[_type]_GFN2_converged.xyz*) and their energies were recorded for comparison (see *1a[_type]_output*).

3) Geometry optimizations at the r<sup>2</sup>SCAN-3c level were performed on the structures obtained in step 2.2. First, optimizations at the NormalSCF convergence criteria were performed, followed by optimizations at the TightSCF convergence criteria. Initially, no convergence could be reached via this method for 1a and 1a_gamma. In both cases, well-converged minima were obtained by restarting from the converged structure of a different diastereomer (with adjusted nitrogens).

   The *ORCA_NormalSCF.in* file contains all the keywords applied during the geometry optimizations performed at the NormalSCF level:
   ```
   ! r2SCAN-3c OPT NORMALSCF def2/J
   %maxcore 8000
   %PAL NPROCS 20 END

   *xyzfile 5 1  GFN2.xyz
   ```

   The *ORCA_TightSCF.in* file contains all the keywords applied during the geometry optimizations performed at the TightSCF level:
   ```
   ! r2SCAN-3c OPT TIGHTSCF def2/J
   %maxcore 20000
   %PAL NPROCS 10 END

   *xyzfile 5 1  1a[_type]_DFT_NormalSCF.xyz
   ```
   
   Well-converged minima were obtained for each type (*1a[_type]_DFT_TightSCF.xyz*) and their energies were recorded for comparison (see *output*).

## 3. Investigation of relative orientations of the phenyl rings
1) New geometries were created for diastereomer 1a_alpha by altering the positioning of the phenyl rings on the arms of the Li<sub>4</sub>L<sub>2</sub> structures using Avogadro. After a series of geometry optimizations (both GFN2-xTB and r<sup>2</sup>SCAN-3c), lower energy gemeotries were found where the phenyl rings where rotated into a parallel configuration. This resulted in a structure where all five arms contained the new arrangement of phenyls (*1a_alpha_app_input.xyz*).

2) New geometries with this new phenyl arrangement ("app" for "all-phenyls-parallel") were created for the other four diastereomrs by altering rotating the Nitrogen atom around one pyrrole ring using Avogadro (*1a_app_input.xyz*, *1a_beta_app_input.xyz*, *1a_gamma_app_input.xyz* and *1a_delta_app_input.xyz*).

3) Geometry optimizations at the r<sup>2</sup>SCAN-3c (TightSCF optimization level) were performed on the structures created in step 3.1 and 3.2 using ORCA, analogous to the calculations perfomed in step 2.3. The *ORCA.in* files contained the following lines:

   ```
   ! r2SCAN-3c OPT TIGHTSCF def2/J
   %maxcore 20000
   %PAL NPROCS 20 END

   *xyzfile 5 1  1a[_type]_app_input.xyz 
   ```

   Well-converged minima were obtained *1a[_type]_app_DFT_TightSCF.xyz* and their total energies were recorded for comparison (see *output*).
