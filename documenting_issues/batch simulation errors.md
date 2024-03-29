# batch simulation errors

- before running: substitute lines in [init.py](http://init.py) w/: 


      `simConfig, netParams = sim.readCmdLineArgs(simConfigDefault='tut8_cfg.py', netParamsDefault='tut8_netParams.py')`

      `simulationsim.createSimulateAnalyze(netParams=netParams, simConfig=simConfig)`

  If not working, substitute  `simulationsim.creatSimulateAnalzye` to `sim.creatSimulateAnalzye`

- make sure `type` is set to `mpi_bulletin` and not `mpi`:

      b.batchLabel = 'tauWeight'
        b.saveFolder = 'ca3attempt_data'
        b.method = 'grid'
        b.runCfg = {'type': 'mpi_bulletin',
                                'script': 'init.py',
                                'skip': True}


- When specifying values to change (here `connWeight`, `delay` and `loc`): 

     **'batch.py' should look like this** :
      
      def batchWeight():
        # Create variable of type ordered dictionary (NetPyNE's customized version)
        params = specs.ODict()

        # fill in with parameters to explore and range of values (key has to coincide with a variable in simConfig)
        params['connWeight'] = [0.4e-3, 0.9e-3, 1.4e-3]
        params['delay'] = [2, 4, 6]
        params['loc'] = [0.25, 0.5, 0.75]
        
      
      
     When I set the values to explore in `cfg.py`, the simulation ignored them and ran with the values in `params` in `batch.py`. Therefore the values in      `cfg.py` should match those in `batch.py`: empty lists for the values in `cfg.py` will break `init.py`.

     **If running batch.py gives this error:**
  
             synMechs = [
             File "/Users/irenebernardi/opt/anaconda3/envs/netpyne/lib/python3.9/site-packages/netpyne/cell/compartCell.py", line                   1676, in <listcomp>
             synLabel=params['synMech'], secLabel=synMechSecs[i], loc=synMechLocs[i], preLoc=params['preLoc']
            synLabel=params['synMech'], secLabel=synMechSecs[i], loc=synMechLocs[i], preLoc=params['preLoc']
            IndexError: list index out of range
            IndexError: list index out of range
            synLabel=params['synMech'], secLabel=synMechSecs[i], loc=synMechLocs[i], preLoc=params['preLoc']
            IndexError: list index out of range
            synLabel=params['synMech'], secLabel=synMechSecs[i], loc=synMechLocs[i], preLoc=params['preLoc']
            IndexError: list index out of range 

  The correct mod compilation may fix it (see [here](https://github.com/irenebernardi/GSoC23/blob/main/documenting_issues/order%20of%20batch%20commands.md)). 




Overall, check that cfg.json looks right and run `print(netParams.connParams())`

     


