# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format: 
**MAJOR.MINOR.PATCH**, eg. 1.0.1

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

### Considerations with the Terraform CLI changes
The Terraform CLI installation instructions may have changed. So refer to the latest install CLI instructions via Terraform Documaentation and change the scripting for install.

[Installl Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Considerations for Linux Distributions
This project is built against Ubuntu. Please consider checking your Linux Distribution and check accordingly to you distribution needs.

[How to check OS version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

```sh
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

#### Refactoring into Bash Scripts

Create a bash script to install the Terraform CLI.

The bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod Task File([.gitpod.yml](.gitpod.yml)) tidy.
- This makes it easier to debug and execute manually Terraform CLI install
-This allows better portability

#### Shebang

Shebang is used to indicate the interpreter that should be used to execute the script.

ChatGPT recommended this format for bash: `#!/usr/bin/env bash`

- For portability for different distributions
- Will search the user's PATH for the bash executable

#### Execution Considerations

When executing the bash script we can use the `./` shorthand notation to execute the bash script.

eg. `./bin/install_terraform_cli`

If we are using a script in .gitpod.yml we need to point the script to a a program to interpret it

eg. `source ./bin/install_terraform_cli`

#### Linux Persmissions Considerations

In order to make our bash scripts executable we need to change linux permission for the fix to be executable

```sh
chmod u+x ./bin/install_terraform_cli 
```

alternatively:

```sh
chmod 744 ./bin/install_terraform_cli 
```

https://en.wikipedia.org/wiki/Chmod

### Github Lifecycle (Before, Init, Command)

We need to be careful when using the Init because it will not rerun if we restart an existing workspace

https://www.gitpod.io/docs/configure/workspaces/tasks

### Working with Environment Variables

We can list out all environmental variables(env vars) using the `env` command.

We can filter specific env vars using the grep commmand eg. `env | grep AWS_`

#### Setting and unsetting env vars

In the terminal we can set env vars using `export HELLO='world'`

In the terminal we can unset env vars using `unset HELLO`

We can set an env var temporarily when running a command using

```sh
HELLO='world' ./bin/print_message
```

Within a bash script we can set env vars leaving out the export keyword

```sh
#!/usr/bin/env bash
HELLO='WORLD'

echo $HELLO
```

## Pinting environment variables

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

VSCODE is unaware of env vars in another terminal that you have set in another window.

To persist env vars accross all fututr bash terminal set env vars in your bash profile.

#### Persisting Env Vars in Gitpod

To persist env vars in gitpod store them in Gitpod Secrets Storage.

```
gp env HELLO='world
```
Env vars can also be set in `.gitpod.yml`, but this can only contain non-sensitive vars.

###AWS CLI Installation

AWS CLI is installed for the project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting Started Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

[AWS CLI ENV VARS](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials is configured correctly by running the following command

```sh
aws sts get-caller-identity
```

if it is succesful you should see a json payload return that looks like this:

```
{
    "UserId": "AKIAIOSFODNN7EXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/terraform-beginner-bootcamp"
}```

We will nee to generate AWS CLI credentials from IAM user in order to use the AWS CLI