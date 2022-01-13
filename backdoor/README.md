# Backdooring Docker 🚪

In the subheadings below you'll find commands/guides for working with the backdooring in this project.

## Vagrant

We are using [Vagrant](https://www.vagrantup.com/) as a common develoment environment.

Outlined below are some commands for working with our VM.
The _commands assume_ that your are running them from the _root of the project_.
Additionally, the commands assume that you are running a single Vagrant VM.

To setup the Vagrant VM run the following the root of the project:

	vagrant up

To interact with the Vagrant VM you can SSH into it.
Vagrant has a convenience command for this:

	vagrant ssh

A Vagrant VM shares the folder that it's corresponding Vagrantfile exists in.
This means that the source code of this whole project can be found inside of the Vagrant VM.
By default, the shared folder is found under `/vagrant` in the VM.

The Vagrant VM requires that some commands are run as superuser (`sudo`).
There is no password setup for this, so `sudo` works out of the box.

Do not be afraid to run anything as `sudo` inside of the VM, as you can always create a new one with:

	vagrant destroy
	vagrant up

**EXCEPTION:** if you delete or modify files under `/vagrant` inside of the VM, the changes will be reflected on your own machine.

## Building Docker

Although you can run the following commands on your own machine, this is _heavily_ discouraged.
If you decide to do so, you might end up with a very malformed version of Docker on your own system.
Instead, run the following commands inside of a Vagrant VM, see above.
The commands assume that you are running Docker on your system as a SystemD service.

To compile and install your modifications to this project, run:

	make install

This command compiles the project and installs it on the system.

After installing with the command above, you can run the newly created, modified version of the Docker daemon, `dockerd`:

	make dockerd

This command stops the running Docker SystemD service and starts the modified Docker daemon in debug mode.

To get your SystemD service up and running again, stop the above command and run:

	make daemon

Once you have your modified Docker daemon running, you can start creating Docker containers.
First, build the container from the `Dockerfile` in this very folder with:

	make build

Once the build is completed successfully you can enter your newly create container with:

	make sh

### When running the modified Docker daemon

When you are running the modified Docker daemon (see above on how to create this) you will see a lot of debug information.
Information related to backdoor related changes are prefixed with `[BACKDOOR]` for easy filtering.
