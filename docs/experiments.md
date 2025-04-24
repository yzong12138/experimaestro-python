# Running experiments

The main class is `experimaestro.experiment`


::: experimaestro.experiment

When using the command line interface to run experiment, the main object
of interaction is the `ExperimentHelper`:

::: experimaestro.experiments.cli.ExperimentHelper

## Experiment services

::: experimaestro.scheduler.services.Service
::: experimaestro.scheduler.services.WebService
::: experimaestro.scheduler.services.ServiceListener


## Experiment configuration

The module `experimaestro.experiments` contain code factorizing boilerplate for
launching experiments. It allows to setup the experimental environment and
read ``YAML`` configuration files to setup some experimental parameters.

This can be extended to support more specific experiment helpers (see e.g.
experimaestro-ir for an example).

`ConfigurationBase` should be the parent class of any configuration.

::: experimaestro.experiments.ConfigurationBase

### Example

An `experiment.py` file:

```py3
    from experimaestro.experiments import ExperimentHelper, configuration, ConfigurationBase

    @configuration
    class Configuration(ConfigurationBase):
        #: Default learning rate
        learning_rate: float = 1e-3

    def run(
        helper: ExperimentHelper, cfg: Configuration
    ):
        # Experimental code
        ...
```

With `full.yaml` located in the same folder as `experiment.py`

```yaml
    file: experiment
    learning_rate: 1e-4
```

The experiment can be started with

```sh
    experimaestro run-experiment --run-mode normal full.yaml
```

### experiments run mode

when using `experimaestro run-experiment`, we can have three run options, `dry-run`,`generate` and `normal`. 

#### dry-run

`dry-run` is an option that can be used to show a list of the task that related to the experiment file, including their dependency. 
Running with this option will NOT launch the corresponding task but will be quite useful to check the dependency between the task and
the verifing the hyperparameters pass to each task is valid.

#### generate

`generate` is an option which could show **and generate** the python code that corresponding to each task to the experiment file. The python
code will be generated at the experimaestro working directory, together with a `param.json` indicating the hyperparameters related to this task.
This is an option that used for debug a single task: We can generate the python code first and then launching the single task with 
```sh
python ....
``` 
to debug without running other tasks. We note that the task could only ran if all the depended task are finished. 

### Other options

The Python path can be set by the configuration file, and module be used instead
of a file:

```yaml
    # Module name containing the "run" function
    module: first_stage.experiment

    # Python paths relative to the directory containing this YAML file
    pythonpath:
        - ../src
```

### Common handling

::: experimaestro.experiments.cli
