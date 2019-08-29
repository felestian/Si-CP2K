Phonon band structure calculation using CP2K and Phonopy
********************************************************
Phonon band structure for bulk silicon
========================================================


See https://www.cp2k.org/exercises:2018_uzh_cmest:phonon_calculation

As a first example we are going to reproduce the text book band structure for bulk silicon.
Take the following CP2K input file - Si.inp

Now let Phonopy parse the CP2K input file and have it generate a 2x2x2 supercell with one 
displacement, by running the following command:

$ phonopy --cp2k -c Si.inp -d --dim="2 2 2"

which should give you the following additional files:

$ ls
disp.yaml  phonopy_disp.yaml  Si.inp  supercell-001.inp  supercell.inp

supercell.inp is a skeleton with only the newly generated supercell, but without the displacement, 
while supercell-001.inp also contains the displacement of one Silicon atom and is therefore the 
calculation we need to run.

You therefore have to extend supercell-001.inp with the rest of the missing calculation 
parameters from the original input file Si.inp. Also make sure that you change project name to Si-supercell-001:

- Copy KIND section, METHOD section and DFT section.
- In global section, edit\add 'PROJECT Si-supercell-001'.
- Run
	$ cp2k.popt -i supercell-001.inp -o Si-supercell-001-forces-1_0.xyz

After running CP2K, you should get the following output file in addition to the usual CP2K output: Si-supercell-001-forces-1_0.xyz.

Now run Phonopy on this output file to generate the force set (afterwards found in a file called FORCE_SETS):

$ phonopy --cp2k -f Si-supercell-001-forces-1_0.xyz

… and run Phonopy again to get the band structure:

$ phonopy --cp2k -c Si.inp -p --dim="2 2 2" --pa="0 1/2 1/2 1/2 0 1/2 1/2 1/2 0" --band="1/2 1/2 1/2 0 0 0 1/2 0 1/2"

This command is supposed to open a window with the plot on your local machine. But this works only if you have setup the X11-Forwarding as described at the beginning of the lecture. Should this not work, then your best chance is to install the phonopy and cp2k-tools package on your local machine and run the last command on your local machine after having transferred all files in the current project folder to your local machine. Phonopy does not call CP2K directly so CP2K does not have to be installed.

Compare the generated plot to plots usually found in literature. Which special points have been specified in the above command? Change them to plot the path 
Γ − X − K − Γ − L instead.

