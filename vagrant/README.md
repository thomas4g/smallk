<h1 id="vagrant"> Quickstart: Vagrant Virtual Machine </h1>

Installing SmallK into a virtual machine (OSX, Linux, Windows) is intended for those who are not doing development and/or do not have a reason to do the full installation on Linux or OSX outlined in sections III.2 to III.4 (Windows full installation coming soon).

The complete stack of software dependencies for SmallK as well as SmallK itself can be rapidly set up and configured through use of Vagrant and VirtualBox and the files included in the repository. To deploy the SmallK VM:
The Vagrant install has been tested on Linux Ubuntu 14.04, Mac OSX Mavericks 10.9, and Windows 7.

**1.** Install [Vagrant](http://www.vagrantup.com/downloads.html) and [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

**2.** ’git clone’ the [smallk_data](https://github.com/smallk/smallk_data) repository. This will be important for testing the installation and starting to work with SmallK. This directory will be synced with a directory of the same name in the VM.

_[Note: For Windows, ensure that you have a VirtualBox version >= 4.3.12. After installing Vagrant, you may need to log out and log back in to ensure that you can run vagrant commands in the command prompt.]_

**3..** From within the vagrant/ directory in the repository, run:
		
		vagrant up
		
This can take as long as an hour to build the VM, which will be based on a minimal Ubuntu 14.04 installation. The VagrantFile can be customized in many ways to change the specifications for the VM that is built. See more information [here](http://docs.vagrantup.com/v2/). The default configuration provides the VM with 4 GB of memory and 2 CPUs. Increasing these allocations will improve the performance of the application. This can be done by modifying this line in the Vagrantfile:

		vb.customize ["modifyvm", :id, "--memory", "4056", "--cpus", "2"]

After ‘vagrant up’ has completed, the SmallK library will have been built and the smallk_data directory, cloned as in **2.** above, will have been synced into the VM.
<br>[--back to top--](#top)

**4..** Once the VM has been built, run:

		vagrant ssh

_[Note: For Windows, you will need an ssh client in order to run the above command. This can be obtained via [CygWin](https://www.cygwin.com/), [MinGW](http://sourceforge.net/projects/mingw/files/), or [Git](http://git-scm.com/downloads). If you would like to use PuTTY to connect to your virtual machine, follow [these](https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Connect-to-Your-Vagrant-Virtual-Machine-with-PuTTY) instructions.]_

This will drop you into the command line of the VM that was just created. Note that the *bootstrap.ssh* file can be modified to perform various tasks when going to the vagrant command line session. Customize bootstrap.sh to set up a custom install of libsmallk as required.

From there, you can navigate to /home/vagrant/libsmallk-<version>, e.g., libsmallk-1.6.0, and run

		make check DATA_DIR=../smallk_data
		
make check will verify your installation was successful. In case you need it, the username/password for the VM created will be vagrant/vagrant.

The command

		sudo make install

will install smallk to the /usr/local/smallk/bin directory. Now add this directory to the path:

		export PATH=/usr/local/smallk/bin:$PATH
[--back to top--](#top)

**5.** To test the installation at the command line, assuming the $PATH variable has been set as above, run

		nmf

This will produce the help out put for the nmf library function:

	Usage: nmf
        --matrixfile <filename>  Filename of the matrix to be factored.
                                 Either CSV format for dense or MatrixMarket format for sparse.
        --k <integer value>      The common dimension for factors W and H.
        [--algorithm  BPP]       NMF algorithms: 
                                     MU:    multiplicative updating 
                                     HALS:  hierarchical alternating least squares
                                     RANK2: rank2 with optimal active set selection
                                     BPP:   block principal pivoting
        [--stopping  PG_RATIO]   Stopping criterion: 
                                     PG_RATIO: Ratio of projected gradients
                                     DELTA:    Change in relative F-norm of W
        [--tol  0.005]           Tolerance for the selected stopping criterion.
        [--tolcount  1]          Tolerance count; declare convergence after this many 
                                 iterations with metric < tolerance; default is to 
                                 declare convergence on the first such iteration.
        [--infile_W  (empty)]    Dense mxk matrix to initialize W; CSV file.
                                 If unspecified, W will be randomly initialized.
        [--infile_H  (empty)]    Dense kxn matrix to initialize H; CSV file. 
                                 If unspecified, H will be randomly initialized. 
        [--outfile_W  w.csv]     Filename for the W matrix result.
        [--outfile_H  h.csv]     Filename for the H matrix result.
        [--miniter  5]           Minimum number of iterations to perform. 
        [--maxiter  5000]        Maximum number of iterations to perform.
        [--outprecision  6]      Write results with this many digits of precision.
        [--maxthreads    4]      Upper limit to thread count. 
        [--normalize  1]         Whether to normalize W and scale H.
                                     1 == yes, 0 == no 
        [--verbose  1]           Whether to print updates to the screen. 
                                     1 == print updates, 0 == silent 


**6.** When you are ready to shut down the VM, run one of the following:

		vagrant suspend # this command will save the current running state
		vagrant halt # this command will gracefully shut down the machine
		vagrant destroy #this command will remove the VM from your machine

If you want to work with the VM again, from any of the above states you can run

		vagrant up
		
again and the VM will be resumed or recreated.
<br>[--back to top--](#top)
