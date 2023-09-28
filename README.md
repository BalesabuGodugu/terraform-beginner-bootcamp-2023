# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format:

 **MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

###  Terraform CLI changes && installation instructions in gitpod

latest terraform installation changes and its resources. 

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)


### Considerations for Linux Distribution


[How To Check OS Version in Linux](
https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

Example of checking OS Version:

```
$ cat /etc/os-release

PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

### Refactoring into Bash Scripts

bash script to install the Terraform CLI.

This bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod Task File ([.gitpod.yml](.gitpod.yml)) tidy.
- This allow us an easier to debug and execute manually Terraform CLI install
- This will allow better portablity for other projects that need to install Terraform CLI.

#### Shebang Considerations

A Shebang (prounced Sha-bang) tells the bash script what program that will interpet the script. eg. `#!/bin/bash`

ChatGPT recommended this format for bash: `#!/usr/bin/env bash`

- for portability for different OS distributions 
-  will search the user's PATH for the bash executable

https://en.wikipedia.org/wiki/Shebang_(Unix)

#### Execution Considerations

When executing the bash script we can use the `./` shorthand notiation to execute the bash script.

eg. `./bin/install_terraform_cli.sh`

If we are using a script in .gitpod.yml  we need to point the script to a program to interpert it.

eg. `source ./bin/install_terraform_cli.sh`

#### Linux Permissions Considerations

In order to make our bash scripts executable we need to change linux permission for the fix to be exetuable at the user mode.

```sh
chmod u+x ./bin/install_terraform_cli.sh
```

alternatively:

```sh
chmod 744 ./bin/install_terraform_cli.sh
```

https://en.wikipedia.org/wiki/Chmod

### Gitpod Lifecycle (Before, Init, Command)

We need to be careful when using the Init because it will not rerun if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/tasks

### working with Env Vars

### env command 

we can list all environment variables using `env` command 

we can filter specific env vars using grep e. `env | grep AWS_`

### setting && un setting Env Vars


in the terminal we can set using `export HELLO='WORLD`

in the terminal we can unset using `unset HELLO`

We can set an env var temeporally when just running a command 

```sh

HELLO=`world` ./bin/print_message

```
within a bash script we can set env without writting export eg.

```sh
#!/usr/bin/env bash

HELLO='WORLD'
echo $HELLO

```

#### Printing Vars

we can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

when you open up new bash terminals in VScode it will not be aware of env vars that you have set in another window. If you

want to Env vars to persist across all future bash terminals that are open you need to set env vars in your bash profile.
eg. `.bash_profile`


#### persist env in gitpod

we can persist env vars into gitpod by storing them in gitpod secrets storage.

```
gp env HELLO = `world`
```

Al future worksspaces launched will set the env vars for all bash terminals opened in those  workspaces.

you can set env vars in teh `gitpod.yml` but this can only contain non-sensitive 


### AWS CLI Installation 

 AWS CLI is installed for the project via bash script [`./bin/install_aws_cli.sh`](./bin/insall_aws_cli.sh)
 [Getting started (AWS CLI)]https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
 [AWS CLI Env vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

 we can check if our aws credentials configured correct by running following command 

 ```sh
 aws sts get-caller-identity
 ```

 after successful , it will give following json response as return 

 ```json

  "UserId": "AIBAQYPQE5W4CVAAGDCOR",
    "Account": "052432802498717",
    "Arn": "arn:aws:iam::057780249237:user/terraform-beginer-group"
 
 
 ```


 ## Terraform Basics

 ### Terraform Registry 

Terraform sources their providers and modules from the Terraform registry which located at [registry.terraform.io](https://registry.terraform.io/)

-** Providers ** is an interface to APIs that will allow to create resources in terraform.
-** Modules **  are a way to make large amount of terraform code modular, portable and sharable

[Randorm Terraform provider](https://registry.terraform.io/providers/hashicorp/random/latest)

### Terraform console

we can see list of all the terraform commands by typing `terraform`


##### Terraform Init

to start new terraform project we wil run `terraform init` to download the binaries for the terraform providers that we will use in the project.


##### Terraform plan

`terraform plan`

this will generate out a changeset, about state of our infrastructure and what will be changed.

we can output this change ie.`terraform plan` to be pass to an apply, but often you can just ignore.

#### Terraform apply 

`terraform apply`

this will run  a plan and pass the changeset to be executed by terraform.Apply should prompt yes or no .

if we want to automatically approve an apply we can provide the auto approve flat eg. `terraform apply --auto-approve`

#### Terraform destroy

`terraform destroy ` will help to destroy the resource in aws


#### Terraform Lock files

`.terraform.lock.hcl` contains the locked versionfor the providers or modules that should be used  with this project.

the terraform lock file **should be commited** to the your github repo.

### terraform state files

`.terraform.tfstate` containing the information about the current state of your infrastructure.

we **shouldn't commited **this file to github. it is containing sensitive infromation 

`.terraform.tfstate.backup` is the prev state file state


### Terraform directory 

`Terraform Directory` typically refers to a directory or folder in your project's file structure where you organize your Terraform configuration files. 

## Terraform Cloud login && Gitpod workspace

please follow the following instructions to login terraform cloud from gitpod workspace.

```
  touch /home/gitpod/.terraform.d/credentials.tfrc.json
  open /home/gitpod/.terraform.d/credentials.tfrc.json

```

please update the file with following instructions

```json

{
  "credentials": {
    "app.terraform.io": {
      "token": "YOUR TERRAFORM CLOUD TOKEN"
    }
  }
}

```

## automate the process using bash script for [bin/generate_tfrc_credentials.sh]