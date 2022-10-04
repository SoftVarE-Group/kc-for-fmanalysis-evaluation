# Empirical Evaluation Framework for Applying Knowledge Compilers to Feature Models
Replication package for evaluation of AMAI submission "On the Benefits of Knowledge Compilation for Feature-Model Analyses"

## General information
The python benchmark script can be used as it comes. Nevertheless, we highly recommend using the provided docker image (https://github.com/users/SundermannC/packages/container/kcfm23/44317965?tag=eval) because it is easier to use and for our results we also used docker.
Furthermore, in `solvers/` the tools are provided as built binaries and the different run configurations are in `run_configurations/`. We divide those in configurations that include confidential models, those that exclude confidential models, and tests configuration. The tests holds the same configurations as the other two, but only considers one model (i.e. Smarch/toybox_0_7_5), runs that model only once, and the output of the tools gets printed onto stdout.

## How to run

For both ways you either have to specify a run configuration (.json or .yaml) file, respectively a folder that contains one or multiple run configurations or use the provided configurations. We provide exclusively .yaml files.

### Run benchmark via Docker
First, if not done already you have to install docker. You can find information on how to build Docker [here](https://docs.docker.com/get-docker/).

Second, we have to pull the image using docker. Note that it might be necessary (depending on your setup) to use `sudo` for Docker.
```
docker pull ghcr.io/sundermannc/kcfm23:eval
```

Now we are ready to run a container with one or multiple run configurations. For instance, the following example starts a docker container from the image ```rp:nc-latest```, with the name test, allocates a tty for the container process (-it flags), and runs all run configurations that are included in the test directory and its subdirectories.
```
docker run --name bdd -d ghcr.io/sundermannc/kcfm23:eval run_configurations/bdd.yaml
docker run --name sdd -d ghcr.io/sundermannc/kcfm23:eval run_configurations/sdd.yaml
docker run --name ddnnf -d ghcr.io/sundermannc/kcfm23:eval run_configurations/ddnnf.yaml
docker run --name eadt -d ghcr.io/sundermannc/kcfm23:eval run_configurations/eadt.yaml
```


You can get the result data by copying it from the container onto your system. Therefore, you can use the following command which accesses the test container and copies the results directory into the ```DET_PATH_ON_HOST```. This can simply be the current working directory (i.e. ./).
```
docker cp bdd:benchmark/results DEST_PATH_ON_HOST
```

### Alternative way to run the benchmark (not recommended)
In general, an experiment specified by a .json or a .yaml file can be executed with Python directly. Nonetheless, we recommend using docker because when using python you have to make sure to install all dependencies on your own and meet all requirements as mentioned in the dockerfile.

For instance, the following command executes all run configurations in the test directory and its subdirectories.
```
python3 run.py run_configurations/bdd.yaml
```
