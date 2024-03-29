# How the shell script works: 
When the script reaches the mpi line: `time mpiexec $ (hostname) -n $NSLOTS nrniv -python -mpi batch.py` it starts a subprocess: a new process from the main process that is the submit script. This is for parallel execution of the python script that is the batch file in this case. All of the jobs that spawn from this are sent to each node.
The batch run (just `python batch.py`) creates a shell script per se , so an actual shell submission isn’t really necessary but useful to validate that job submission works on Downstate HPC.

Script should look like this: 

      #!/bin/bash

      #$ -N test
      #$ -cwd
      #$ -pe smp 5
      #$ -l h_vmem=128G

      # see the submission script
      cat $SGE_O_WORKDIR/submit.sh

      source ~/.bashrc
      time mpiexec $ (hostname) -n $NSLOTS nrniv -python -mpi batch.py

- A generic output file containing info on how mpiexec works probably means the last line of the submit script isn't set correctly.
- TODO: Add info on mod compilation once solved


Other useful info:
- Always check that the output file has a longer "user" simulation time than a "real" simulation time.
- batch run type should be set to `SGE`.
- setup instructions and commands [here](https://docs.google.com/document/d/1VwjgOICa2Pj7pE_TODhQCaztmP07qi_C-pSCzCQ5o38/edit#heading=h.cabvm0d2hgwb).
