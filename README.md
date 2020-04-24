# Conda Environment Guide

The purpose of this guide when the [official one](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#updating-an-environment) by Conda exists, is to summarize key aspects that are required, while highlighting some gotchas experienced while using that guide.

If a particular feature of Conda is missing in this guide, then please refer to the official guide or submit an issue addressing it.

> **NOTE:** The commands in this document have only been tested on Windows using Powershell. On other platforms and environments, you mileage may vary.

# Contents

- [Conda installation](#conda-installation)
  - [Installing dependencies](#installing-dependencies)
- [Create new environment](#create-new-environment)
- [Export existing environment](#export-existing-environment)
  - [Known issues and fixes](#export-known-issues)
- [Restore from environment](#restore-from-environment)
  - [Known issues and fixes](#restore-known-issues)
- [Update environment](#update-environment)

# Conda installation <a name="conda-installation"></a>

Install Conda using the official website: [https://www.anaconda.com/](https://www.anaconda.com/)

## Installing dependencies <a name="installing-dependencies"></a>

> **NOTE:** Install dependencies inside of particular environments to avoid versioning conflicts.

Install the dependencies from the [Conda Cloud](https://anaconda.org/) using:

`conda install <pkg-name>` or check Conda Cloud for installation details.

> **NOTE:** Sometimes the format of this installation can look different, but this works for most cases as long as the package exists. Check Conda Cloud to be sure.

When a package is not on Conda Cloud, then it can be installed by using `pip` by:

`pip install <pkg-name>`

> **NOTE:** I have read that using `pip` and `conda` side by side can potentially lead to conflicts, but I have not experienced this in my use and testing. I do try to use Conda wherever I can just in case.

# Create new environment <a name="create-new-environment"></a>

The best (not too inelegant and completely serviceable) method I have found is to create an evironment for every project. This would make managing many, many environments installed in the Conda root directory extremely difficult. A solution to this is to create a folder called `envs` in the root of the project, and create the environment there.

Create new with no prior dependencies in the conda root directory:

`conda create -n nameOfEnv`

Create new with prior dependencies link Tensorflow:

`conda create -n nameOfEnv tensorflow` or
`conda create -n nameOfEnv tensorflow-gpu`

You can specify the location of this new environment by, but then you won't be able to give it a name:

`conda create tensorflow -p .\envs`

> **NOTE:** The prefix `.\envs` creates a folder of the name `envs` in the current directory with all the dependencies for the environment.

Enter, or activate, the environment if installed in the Conda root directory by:

`conda activate nameOfEnv`

Or if installed in the `envs` folder, activate it using:

`conda activate .\envs`

# Export existing environment <a name="export-existing-environment"></a>

To export the environment details to a file so that someone else can create an environment identical to it:

`conda env export > environment.yml`

## Known issues and fixes <a name="export-known-issues"></a>

_Issue:_ When I exported this, the file encoding of `environment.yml` was UTF-16 and Conda expects it to be UTF-8 in order for it to read or parsed correctly.

**Fix:** To change the file encoding I used VSCode, like in the [solution](https://github.com/conda/conda/issues/7381) of the same issue someone posted on another project, to save the file with a new encoding in UTF-8.

> The solution is at the bottom of the issue.

# Restore from environment <a name="restore-from-environment"></a>

To setup the same environment as the one required by the project in a folder called `envs` in the root of the project, create a new conda environment using:

`conda env create -f .\environment.yml -p .\envs`

And activate it using:

`conda activate .\envs`

## Known issues and fixes <a name="restore-known-issues"></a>

**Note:** The encoding of the file `environment.yml` has to be UTF-8 and not UTF-16. If you are getting the issue similar to `Issue#1` then this might be why. To change it, follow the solution referenced on the issue.

# Update environment <a name="update-environment"></a>

If there have been changes to the `environment.yml` file, install the additional or updated dependencies using:

`conda env update -p ./env -f environment.yml --prune`

> **NOTE:** This has not been tested yet, and this section will be updated when I do.

**TODO: Complete section with details about how to deal with adding new dependencies after creating the `environment.yml` file.**
