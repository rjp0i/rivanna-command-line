### Rivanna from the command line
_A Mac-targeted guide for [StatLab](https://data.library.virginia.edu/statlab/)_  
Author: [Jacob Goldstein-Greenwood](https://github.com/jacob-gg)  
Last updated: 2022-09-23  

---
**Log in with SSH**  

`ssh -Y <usr>@rivanna.hpc.virginia.edu`

---
**Transfer files**  

_Note: This section will be updated with SFTP/rsync transfer instructions._

_Run SCP from the local (not in SSH)._  

_Local to remote:_  
`scp localfile <usr>@rivanna.hpc.virginia.edu:/path/file`  
`scp -r localdir <usr>@rivanna.hpc.virginia.edu:/path/dir`

_Remote to local:_  
`scp <usr>@rivanna.hpc.virginia.edu:/path/file /targetfile`  
`scp -r <usr>@rivanna.hpc.virginia.edu:/path/dir /targetdir`

Directory info:
- SCP takes the directory it's invoked in as the default path
  - `cd` to relevant directory to make life easy
- Default remote path is `home/<usr>/`
  - `mkdir` in SSH to create remote directory if needed

---
**Slurm script**  

Rivanna receives and manages jobs based on Slurm scripts. Write the Slurm script in bash, and save it as a `.slurm`.

_Note: I recommend using a text editor other than TextEdit (e.g., Atom) on Mac, as TextEdit seems to have trouble saving files as `.slurm` without coercing to `.rtf`._

Sample Slurm script to execute a serial (single-core) R script on Rivanna:
```
#!/bin/bash
#SBATCH -A <alloc>
#SBATCH --time=04:00:00
#SBATCH -N 1
#SBATCH --mem 64000
#SBATCH -p standard
#SBATCH -o output.out

module purge
module load goolf/7.1.0_3.1.4 R/4.0.3

Rscript script.R
```

Key options:  
`-A`, `--account`: allocation to use  
`-t`, `--time`: max time to allot  
`-N`, `--nodes`: nodes  
`-n`, `--ntasks`: tasks  
`-M`, `--mem`: RAM in mb  
`-p`, `--partition`: partition (standard, dev)  
`-o`, `--output`: output file (optional; default name will be used otherwise)

---
**Execute script on Rivanna**  

`sbatch slurmscript.slurm`

---
**Ad hoc Rivanna CL commands**  

`allocations -a <alloc>` # view allocation details  
`squeue --user <usr>` # view user's current jobs  
`jobq` # info on your current jobs  
`qlist` # info on available queues/partitions  
`qlimits` # info on queue/partition limits  
`sfsq` # info on your scratch (temporary) storage  
`scancel <jobid>` # cancel a job  

---
**Ad hoc R-in-Rivanna CL commands**  

`module purge` # clear modules  
`module load goolf/7.1.0_3.1.4 R/4.0.3` # load R 4.0.3  
`module spider R` # see available R versions  

Once R is loaded in the CL, install packages as usual (`install.packages()`); a prompt will open to select a mirror to download from (e.g., Germany Leipzig).

_Canary: R 4.1.1 on Rivanna seems to have package-handling issues._
