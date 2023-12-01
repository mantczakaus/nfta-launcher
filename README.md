# nfta-launcher

This Python script facilitates running Nextflow tower agent on different platforms including `local` execution, through `slurm` and `pbspro` job schedulers and with recommended configuration for Setonix and GADI supercomputers at Pawsey and NCI. 


## Usage/Examples
You can use this script by either supplying information on the command line via the parameters shown below, or by updating the config.json file and using the `--config config.json` flag. We reccomend using the config file if possible.


To run the agent locally with configured parameters in the config.json file, you can do the following:

```bash
git clone https://github.com/AustralianBioCommons/nfta-launcher
cd  nfta-launcher
nfta-launcher.py --connection-id <conn>
# To run the agent through a slurm job on Setonix try:
# python nfta-launcher.py --connection-id <conn> --platform setonix --config config.json --access-token 
```

An example of running the agent locally with parameters provided on the command line:

```bash
git clone https://github.com/AustralianBioCommons/nfta-launcher
cd  nfta-launcher
nfta-launcher.py --connection-id <conn> --access-token xxxx --work-dir /path/to/dir
# To run the agent through a slurm job on Setonix try:
# python nfta-launcher.py --connection-id <conn> --platform setonix  --access-token xxxx --work-dir /path/to/dir

```


## Parameters

The tool accepts and prioritises parameters in the following:

+ **--platform**: Platform for execution from {local,background,gadi,setonix}. The default is local.
+ **--connection-id**: The connection id for the credential. Should be taken from the workspace on Tower.
+ **--access-token**: The access token should be taken from Tower for each user.
+ **--work-dir**: the Working directory for the agent.
+ **--config**: Optional configuration file similar to `config.json` to be used to customise the run.
+ **--update-agent**: Update `tw-agent` software before running it. The default is `False`.
+ **--user**: the user name on the machine.
+ **--project**: the project code when running on Setonix/GADI.
+ **--log-level**: logger level {`DEBUG`,`INFO`,`WARNING`,`ERROR`,`CRITICAL`}. The default is `INFO`, others are not implemented now.
+ **--log-destination**: The log destination for the agent stdout is default or a file.
+ **--agent-debug-mode**:  Enable agent debug mode for extra tracking information in the logs. default is `False`.
+ **--agent-dir**: The location where to save `tw-agent` software. if does not exist, it will be created.
+ **--hpc-job-conf**: Job configuration to overwrite default configuration in `config.json`.
+ **--job-log**:  path and prefix of output and error Log files for the submitted jobs or the agent outputs. `.err` and `.out` will be added at the end of the file name
+ **-h, --help**: show the help message and exit.

## Details

The tool starts by validating all needed parameters by considering the prioritisation described below.

After all needed parameters are validated, the tool checks if `tw-agent` exists under the `agent-dir`. If not, or if `update-agent` is enabled, the tool will download the the agent to the specified directory under the `agent-dir`.

If the platform is `local`, the tool will run the agent locally and keep printing the agent log.

Otherwise, the tool will build an HPC job to run the agent using the job configs in the configuration. If `hpc-job-conf` is provided, the tool will overwrite the configuration based on the value of this parameter. (Not implemented yet).

If the working directory of the agent is not provided, the tool will use `nfta-wdir` in the current working directory.

If the working directory of the agent does not exist, the tool will create a new directory.

## Parameter prioritisation

The parameter above will be prioritised as follows:

1. Command line arguments passed to the `nfta-launcher.py` have the highest priority.
2. Parameters configured in config file provided by `--config`.
3. Parameters configured in `config.json` file existing in the working directory.
4. parameters configured in `config.json` file existing in the same directory as `nfta-launcher`.
5. `--project`, `--user`, and `--access-token` parameters will be taken from the environment variables if not provided above.
6. `connection-id` and `access-token` will be requested by the script to be entered manually if not provided by any way from above.


## Default configurations for Setonix at Pawsey

Project and users are taken from eh input parameters or using `$USER` and `$PAWSEY_PROJECT` environment variables.

```json
        "work-dir": "/scratch/{project}/{user}/nfta-wdir/",
        "agent-dir": "/software/projects/{project}/nf-tower/",
        "job-config": {
            "job-name": "NF-Tower Agent",
            "partition": "work",
            "time": "24:00:00",
            "ntasks": 1,
            "ntasks-per-node" : 1,
            "cpus-per-task" : 2,
            "mem": "12G"
        }
``` 


## Best way to use the script (Have not thought much about it!)


The best is to copy the `config.json` to somewhere private so no one accesses the `access-token` and `connection-id`. Then run the script either from the same directory or using `--config` parameter. 

## FAQ




## Acknowledgements


## License

[MIT](https://choosealicense.com/licenses/mit/)

