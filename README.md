# industrial_ci
Continuous integration repository for ROS-Industrial

Description
============

This repository contains [CI (Continuous Integration)](https://en.wikipedia.org/wiki/Continuous_integration) configuration that can be commonly used by the repositories in [ros-industrial](https://github.com/ros-industrial) organization (calling them as "**client**" repos). ROS-powered repositories in other organizations can potentially utilize the CI config here too.

(As of November 2015) The CI config in this repository is intended to be used by the client repos by `git submodule` feature. This repo provides configuration for `travis CI`. In client repos you can define the repository-specific checks, in addition to calling the config stored in this repo.

Usage
======

Here are some operations in your client repositories.

First time you define the dependency to this repo
--------------------------------------------------

 1. Run git submodule command.

 ```
CLIENTREPO_LOCAL$ git submodule add https://github.com/ros-industrial/industrial_ci .ci_config
```

 This standard `git submodule` command hooks up your client repository to this repo by the name `.ci_config` (`.ci_config` is hardcoded so you should not change).

 2. Don't forget to activate CI on your github repository (you may do so on https://travis-ci.org/profile/YOUR_GITHUB_USER).

 3. In `.travis.yml` file in your client repo, add the portion below:

 ```
script: 
  - source .ci_config/travis.sh
  #  - source ./travis.sh  # Optional. Explained later
```

That's it.

Apply the changes in this repo (industrial_ci) to the checking in client repos
----------------------------------------------------------------------------------

Maintainers of client repos are responsible for applying the changes in this repos, if they want to stay up-to-date.

 1. Update the SHA key of the commit in this repo. The command below assumes that there's `.gitmodules` file that's generated by `git submodule add` command explained above.

 ```
CLIENTREPO_LOCAL$ git submodule foreach git pull origin master
```

 2. Don't forget to commit the changes the command above makes.

(Optional but recommended) Subscribe to the change in this repo (industrial_ci)
---------------------------------------------------------------------------------

Because of the maintainers' responsibility described above to keep the CI config client repos use up-to-date, [you're encouraged to subscribe to the updates in this repository](https://github.com/ros-industrial/industrial_ci/subscription).

Add repository-specific CI config in addition
----------------------------------------------------------------

Sometimes CI config stored in `industrial_ci` repo may not be sufficient for your purpose. In that case you can add your own config, while you still take advantage of `industrial_ci` repository.

 1. In `.travis.yml` file in your client repo, add the portion below:

 ```
script: 
  - source .ci_config/travis.sh
  - source ./travis.sh
```

 2. Create `travis.sh` file and define the checks you wish to add. NOTE: this `.sh` file you add here is a normal shell script, so this shouldn't be written in `travis CI` grammar.

For maintainers of industrial_ci repository
================================================

While this repository provides CI config that can be used by other repositories, it also checks this repo itself using the same CI config and the simplest package setting. That is why this repo contains the ROS package files and a test (`CMakeLists.txt`, `package.xml`, `.test`).