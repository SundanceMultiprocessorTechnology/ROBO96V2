===============
Getting Started
===============

Example Designs
===============

The example designs for the ROBO96V2 Mezzanine are hosted on Github and
are designed for the Avnet Ultra96 development platform. There are currently two 
examples and they are differentiated by the type of Ethernet MACs used and their 
location in the system.

AXI Ethernet based example
--------------------------

This example design is based on Xilinx's soft (ie. FPGA implemented) MAC,
the AXI Ethernet Subsystem IP, that can be found in the Vivado IP Catalog.
As the MAC is implemented in the FPGA fabric, this example is ideal for 
applications that require some packet processing to be performed in the FPGA.

Requirements
============

In order to use the example designs, you will need the following:

* Windows or Linux PC
* Xilinx Vivado
* Xilinx Vitis
* 1x Ultra96 development platform
* 1x 96B Quad Ethernet Mezzanine

If you want to build PetaLinux for the example designs, you will also need:

* Linux PC or a virtual Linux machine
* PetaLinux Tools

You will also need a CAT-5e Ethernet cable and a link partner, such as a PC with an Ethernet port 
or a network router.

Additionally, you may need to install the Ultra96 board definition files to your Vivado 
installation, and obtain an AXI Ethernet evaluation license if you intend to use that design.

Install Ultra96 board definition files
--------------------------------------

To use the example projects, you must first install the board definition files for the Ultra96 into your Vivado
and Vitis installation. The Ultra96 board definition files are hosted on 
`Avnet's Github repo <https://github.com/Avnet/bdf>`_.

Clone or download that repo, then copy the ``ultra96v1`` and ``ultra96v2`` directories from it to the
following directories on your machine: 
* ``<path-to-xilinx-vivado>/data/boards/board_files``
* ``<path-to-xilinx-vitis>/data/boards/board_files``

AXI Ethernet evaluation license
-------------------------------

If you intend to build the AXI Ethernet based design, you will need to get an evaluation (or full)
license for the Tri-mode Ethernet MAC from Xilinx. You can find instructions for that here:
`Xilinx Soft TEMAC license <http://ethernetfmc.com/getting-a-license-for-the-xilinx-tri-mode-ethernet-mac/>`_

Build instructions
==================

TODO

Build the Vivado and Vitis projects
-----------------------------------

Once you have installed the board definition files, and you have installed the required licenses, then
you can use the sources in this repository to build the Vivado, Vitis and PetaLinux projects. Start by cloning the repo 
or download it as a zip file and extract the files to your hard drive, then follow these steps depending on your OS:

Windows users
^^^^^^^^^^^^^

#. Open Windows Explorer, browse to the repo files on your hard drive.
#. In the Vivado directory, you will find multiple batch files (\*.bat).
   Double click on the batch file of the example project that you would
   like to generate - this will generate a Vivado project.
#. You will be asked to select between Ultra96 v1 and v2. It is important to select the
   correct version of the Ultra96 that you are using. Type 1 or 2 (for v1 or v2)
   and press ENTER. The script will now generate the Vivado project for your board.
#. Run Vivado and open the project that was just created.
#. Click Generate bitstream.
#. When the bitstream is successfully generated, select "File->Export->Export Hardware".
   In the window that opens, tick "Include bitstream" and "Local to project".
#. Return to Windows Explorer and browse to the Vitis directory in the repo.
#. Double click the `build-vitis.bat` batch file. The batch file will run the
   `build-vitis.tcl` script and build the Vitis workspace containing the hardware
   design and the software application. Please refer to the "README.md" file in the Vitis
   subdirectory for instructions on running the software application on hardware.
#. If you are interested in building PetaLinux, you will need to use a Linux machine and
   follow the steps for Linux users below.

Linux users
^^^^^^^^^^^

#. Launch the Vivado GUI.
#. On the welcome page, there is a Tcl console. In the Tcl console, ``cd`` to the repo files on your hard drive
   and into the Vivado subdirectory. For example: ``cd /media/projects/ethernet96/Vivado``.
#. Specify the version of Ultra96 you want to build the project for (v1 or v2) by 
   using one of the following commands: ``set argv {1}`` for v1, or ``set argv {2}`` for v2.
#. In the Vivado subdirectory, you will find multiple Tcl files. To list them, type ``exec ls {*}[glob *.tcl]``.
   Determine the Tcl script for the example project that you would
   like to generate (for example: ``build-ps-gem.tcl``), then ``source`` the script in the Tcl console:
   For example: ``source build-ps-gem.tcl``
#. Vivado will run the script and generate the project. When it's finished, click Generate bitstream.
#. When the bitstream is successfully generated, select "File->Export->Export Hardware".
   In the window that opens, tick "Include bitstream" and "Local to project".
#. To build the Vitis workspace, open a Linux command terminal and ``cd`` to the Vitis directory in the repo.
#. The Vitis directory contains the ``build-vitis.tcl`` script that will build the Vitis workspace containing the hardware
   design and the software application. Run the build script by typing the following command:
   ``<path-of-xilinx-vitis>/bin/xsct build-vitis.tcl``
   Note that you must replace ``<path-of-xilinx-vitis>`` with the actual path to your Vitis installation.
#. Please refer to the "README.md" file in the Vitis subdirectory for instructions on running the software 
   application on hardware.
#. To build the PetaLinux project, follow the steps in the following section.

Build the PetaLinux projects
----------------------------

Once the Vivado project(s) have been built and exported, you can now build the PetaLinux project(s).

.. NOTE:: The PetaLinux projects can only be built on a Linux machine (or virtual Linux machine).

Linux users
^^^^^^^^^^^

#. To build the PetaLinux project, first launch PetaLinux by sourcing the "settings.sh" bash script, 
   eg: ``source <path-to-installed-petalinux>/settings.sh``.
#. Now ``cd`` to the PetaLinux directory in the repo and run the ``build-petalinux`` 
   script. You may have to add execute permission to the script first using ``chmod +x build-petalinux``,
   then run it by typing ``./build-petalinux``.

.. WARNING:: **UNIX line endings:** The scripts and files in the PetaLinux directory of this repository must 
          have UNIX line endings when they are executed or used under Linux. The best way to ensure UNIX 
          line endings, is to clone the repo directly onto your Linux machine. If instead you have copied 
          the repo from a Windows machine, the files will have DOS line endings and
          you must use the ``dos2unix`` tool to convert the line endings for UNIX.

Launch on hardware
==================

Echo server via JTAG
--------------------

#. Open Vitis.
#. Power up your hardware platform and ensure that the JTAG is connected properly.
#. Select "Xilinx Tools->Program FPGA". In the "Program FPGA" dialog box that appears, select the
   "Hardware Platform" that you want to run, this will correspond to name of the Vivado project that
   you built earlier.
#. Click on the software application that you want to run, it should be the one with the postfix "_echo_system".
#. Open the drop-down menu of the "Run" button (play) on the toolbar. Select "Run Configurations", then in the 
   dialog box that appears, double-click on the option
   "Single Application Debug". This will create a new run configuration for the application.
#. Select the new run configuration and click "Run".



PetaLinux via JTAG
------------------

To launch the PetaLinux project on hardware via JTAG, connect and power up your hardware and then
use the following commands in a Linux command terminal:

#. Change current directory to the PetaLinux project directory: ``cd <petalinux-project-dir>``
#. Download bitstream to the FPGA: ``petalinux-boot --jtag --fpga``
   Note that you don't have to specify the bitstream because this command will use the one that it finds
   in the ``./images/linux`` directory.
#. Download the PetaLinux kernel to the FPGA: ``petalinux-boot --jtag --kernel``

PetaLinux via SD card
---------------------

To boot PetaLinux on hardware via SD card:

#. The SD card must first be prepared with two partitions: one for the boot files and another 
   for the root file system.

   * Plug the SD card into your computer and find it's device name using the ``dmesg`` command.
     The SD card should be found at the end of the log, and it's device name should be something
     like ``/dev/sdX``, where ``X`` is a letter such as a,b,c,d, etc. Note that you should replace
     the ``X`` in the following instructions.
   * Run ``fdisk`` by typing the command ``sudo fdisk /dev/sdX``
   * Make the ``boot`` partition: typing ``n`` to create a new partition, then type ``p`` to make 
     it primary, then use the default partition number and first sector. For the last sector, type 
     ``+1G`` to allocate 1GB to this partition.
   * Make the ``boot`` partition bootable by typing ``a``
   * Make the ``root`` partition: typing ``n`` to create a new partition, then type ``p`` to make 
     it primary, then use the default partition number, first sector and last sector.
   * Save the partition table by typing ``w``
   * Format the ``boot`` partition (FAT32) by typing ``sudo mkfs.vfat -F 32 -n boot /dev/sdX1``
   * Format the ``root`` partition (ext4) by typing ``sudo mkfs.ext4 -L root /dev/sdX2``

#. Copy the following files to the `boot` partition of the SD card:
   Assuming the ``boot`` partition was mounted to ``/media/user/boot``, follow these instructions:

   .. code-block:: console
      
      $ cd /media/user/boot/
      $ sudo cp /<petalinux-project>/images/linux/BOOT.BIN .
      $ sudo cp /<petalinux-project>/images/linux/boot.scr .
      $ sudo cp /<petalinux-project>/images/linux/image.ub .

#. Create the root file system by extracting the ``rootfs.tar.gz`` file to the ``root`` partition.
   Assuming the ``root`` partition was mounted to ``/media/user/root``, follow these instructions:

   .. code-block:: console
      
      $ cd /media/user/root/
      $ sudo cp /<petalinux-project>/images/linux/rootfs.tar.gz .
      $ sudo tar xvf rootfs.tar.gz -C .
      $ sync
   
   Once the ``sync`` command returns, you will be able to eject the SD card from the machine.

#. Connect and power your hardware.


Echo Server Example Usage
=========================

Default IP address
------------------

The echo server is designed to attempt to obtain an IP address from a DHCP server. This is useful
if the echo server is connected to a network. Once the IP address is obtained, it is printed out
in the UART console output.

If instead the echo server is connected directly to a PC, the DHCP attempt will fail and the echo
server will default to the IP address 192.168.1.10. To be able to communicate with the echo server
from the PC, the PC should be configured with a fixed IP address on the same subnet, for example:
192.168.1.20.

Ping the port
-------------

The echo server can be "pinged" from a connected PC, or if connected to a network, from
another device on the network. The UART console output will tell you what the IP address of the 
echo server is. To ping the echo server, use the ``ping`` command from a command console.

For example: ``ping 192.168.1.10``