# HPC cheat sheet 
Jonas Weber

## Useful links:

[HPC application](https://www.zim.hhu.de/high-performance-computing.html)

[HPC wiki](https://wiki.hhu.de/display/HPC/Wissenschaftliches+Hochleistungs-Rechnen+am+ZIM)

[screen wiki][screen]

[myJam website][myJam]

[HHU VPN setup](https://www.zim.hhu.de/services-des-zim/netz/netzzugang/vpn-von-zu-hause-ins-uni-netz.html)


## Connect with terminal

Normal login with terminal:

	ssh <username>@hpc.rz.uni-duesseldorf.de

A useful step using Linux is creating an alias so you don't have to type the full HPC address every time you want to connect. This is done in the following way:

Open the following file with an editor of your choice:

	~/.ssh/config

And add the lines:

	Host hilbert
	HostName hpc.rz.uni-duesseldorf.de
	User <username>

This way you can just type:

	ssh hilbert 

## Other ways of connecting

### Mount a directory:

You can mount a HPC directory to your filesystem:

	sshfs <username>@hpc.rz.uni-duesseldorf.de:some/path/on/the/HPC some/path/on/your/system

Example:

	sshfs <username>@hpc.rz.uni-duesseldorf.de:/gpfs/scratch/<username> Documents/hilbert3/

Or use [Filezilla](https://filezilla-project.org/).

Also, look at [this HPC wiki post](https://wiki.hhu.de/display/HPC/Filesysteme+mounten).

### Graphical Session

Request one [here](https://view-2018.hpc.rz.uni-duesseldorf.de/enginframe/vdi/vdi.xml?_uri=//com.enginframe.interactive/list.sessions).


## Your home directory

Work in `/gpfs/scratch/<username>`.



## Modules

You can load specific software with modules:
	
	module load <module-name>

List of all modules:

	module avail

For conda:

	module load Miniconda/3

For conda with Snakemake (different miniconda module):
	
	module load Miniconda/3_snakemake Snakemake/5.10.0


## screen

One can use [screen] to start a new screen session

 	screen                   # without name or
	screen -S <sessionname>  # with name

Use `ctrl+a+d` to detach from this screen. 

Reconnect using:
	
	screen -r                # without name or
	screen -r <sessionname>  # with name

## Interactive session

To get an interactive session use the `qsub` command when connected to the HPC. Make sure to first call `screen` so you can detach and reconnect from/to your session. You can monitor your jobs with [myJam]. You might need to catch the output in some logfile. Tips for this can be found below. 

	qsub -A <project-name> -I -l select=1:ncpus=<number>:mem=<number>G -l walltime=<h>:<mm>:<ss>

Example:

	qsub -A albi -I -l select=1:ncpus=4:mem=10G -l walltime=3:00:00


Normal Workflow:
	
	ssh hilbert
	screen
	qsub -A <project-name> -I -l select=1:ncpus=<number>:mem=<number>G -l walltime=<h>:<mm>:<ss>
	cd /gpfs/scratch/<username>
	module load ...
	<some command> |& tee -a log.txt

Also look at [this HPC wiki post](https://wiki.hhu.de/display/HPC/Entwicklungs-Server)



## Other useful stuff

### Display sizes of all files and dirs
	
	du -bsh * 

### Find all files of certain form
	
	find . -name "foo*" 

### Show current path

	pwd

### Handling stdout and stderr

	echo "42" >> log.txt         # stdout -> log
	echo "42" | tee -a log.txt   # stdout -> log & stdout
	echo "42" &>> log.txt        # stdout & stderr -> log
	echo "42" |& tee -a log.txt  # stdout & stderr -> log & stdout





[screen]: https://wiki.ubuntuusers.de/Screen/
[myJam]: myjam3.hhu.de

