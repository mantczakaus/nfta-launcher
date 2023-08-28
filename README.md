# nfta-launcher

This Python script facilitates running Nextflow tower agent on different platforms including local execution, through `slurm` and `pbspro` job schedulers and with recommended configuration for Setonix and GADI supercomputers at Pawsey and NCI.



## parameters

The tool accepts and prioritises parameters in the following:

+ **--platform**: Platform for execution from {local,gadi,setonix}. The default is local.
+ **--connection-id**: the connection id for the credential. Should be taken from the workspace on Tower.
+ **--access-token**: The access token should be taken from Tower for each user.
+ **--work-dir**: the Working directory for the agent.
+ **--config**: Optional configuration file similar to `config.json` to be used to customise the run.
+ **--update-agent**: Update `tw-agent` software before running it. The default is `False`.
+ **--user**: the user name on the machine.
+ **--project**: the project code when running on Setonix/GADI.
+ **--log-level**: logger level {DEBUG,INFO,WARNING,ERROR,CRITICAL}. The default is `INFO`. others are not implemented now.
+ **--log-destination**: The log destination for the agent stdout is default or a file.
+ **--agent-debug-mode**:  Enable agent debug mode for extra tracking information in the logs. default is `False`.
+ **--agent-dir**: The location where to save `tw-agent` software. if not exist, it will be created.
+ **--hpc-job-conf**: Job configuration to overwrite default configuration in `config.json`.
+ **--job-log**:  path and prefix of output and error Log files for the submitted jobs. `.err` and `.out` will be added at the end of the file name
+ **-h, --help**: show the help message and exit.


## Parameter prioritisation

The parameter above will be prioritised as follows:

1. Command line arguments passed to the `nfta-launcher.py` have the highest priority.
2. Parameters configured in config file provided by `--config`.
3. Parameters configured in `config.json` file existed in the working directory.
4. parameters configured in `config.json` file existed in the same directory as `nfta-launcher`.
5. `--project`, `--user`, and `--access_token` parameters will be taken from the environment variables if not provided above.
6. `connection-id` and `access-token` will be requested by the script to be entered manually if not provided any way from above.


## Usage/Examples

To run the agent locally with configured parameters in the config.json file, you can do the following:

```bash
git clone https://github.com/AustralianBioCommons/nfta-launcher
cd  nfta-launcher
nft-launcher/nfta-launcher.py --connection-id <conn>
# To run the agent through a slurm job on Setonix try:
# nft-launcher/nfta-launcher.py --connection-id <conn> --platform setonix
```



## FAQ




## Acknowledgements


## License

[MIT](https://choosealicense.com/licenses/mit/)

