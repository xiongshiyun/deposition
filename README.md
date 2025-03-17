# deposition
deposition scripts for GPUMD

1. Introduction
The deposition script is built on Python 3 and Shell scripting, and it aims to simulate the deposition process of atoms and atomic clusters using the GPUMD software package.
If you use this method in your published work, please cite the following paper: Li Y, Guo Y, Xiong S, Yi H, Enhanced heat transport in amorphous silicon via microstructure modulation. International Journal of Heat and Mass Transfer, 2024, 222: 125167.
To execute this script, users need to generate the required in addition to generating the GPUMD input file and replicate the contents of the ‘./main’ directory. Afterward, you should provide the necessary parameters in ‘parameter.in’ and configure the cluster position files in ‘./main/cluster’. Once completed, execute the 'submit.sh' file for simulation. Notes: please set the environmental parameters correctly according to your local machine.

2. Parameters
To ensure accurate simulations, the following parameters in ‘parameter.in’ must be configured:
path: Specify the path to the GPUMD software package.
cycle: Define the total number of deposition cycles (Default is 5).
species: Specify the number of deposition groups (must correspond to the files in ‘./cluster’).
number: Define the total number of atomic clusters to be deposited in each cycle (Default is 15).
velocity: Set the deposition velocity of clusters along the z-axis. (Default is 0.005 Å/fs, note: the deposition must be along the z-direction, i.e., atoms or clusters are deposited on the xy plane).
group: Define the grouping method for deposited atoms (Default is 3, 0: fixed substrate atoms; 1 & 2: thermostats).
x_min&x_max: Define the deposition range along the x-axis (Default is 0 to the box edge along the x-axis).
y_min&y_max: Define the deposition range of along the y-axis (Default is 0 to the box edge along the y-axis).
h_min&h_max: Define the initial height range for cluster deposition (z-axis).
cutoff: Set the minimum distance between two clusters (Default is 7 Å).
h_cutoff: Define the maximum height (z-axis) for cluster deposition (Atoms exceeding this value will be deleted).
Note: Avoid depositing too many atoms at once to prevent the program from freezing.
Upon configuring ‘parameter.in’, you must define the atomic positions for each cluster in the ‘./clusters’ directory using files named ‘0.txt’, ‘1.txt’, ‘2.txt’, etc. For example, to simulate the deposition of a single Si atom, you can input the following data into ‘0.txt’:
‘Si     0.00000    0.00000    0.00000     28.0855’
The first column is atomic type, the second to the fourth columns are the x, y, and z coordinates, and the fifth column corresponds to the atomic mass.
If one wants to continue to incorporate a Si unit cell for deposition, you can proceed to input the following data in ‘1.txt’:
‘Si     0.00000    0.00000    0.00000     28.0855
Si     1.35767    1.35767    1.35767     28.0855
Si     0.00000    2.71535    2.71535     28.0855
Si     1.35767    4.07302    4.07302     28.0855
Si     2.71535    0.00000    2.71535     28.0855
Si     4.07302    1.35767    4.07302     28.0855
Si     2.71535    2.71535    0.00000     28.0855
Si     4.07302    4.07302    1.35767     28.0855’
Please note that the number of clusters should correspond to the ‘species’ in ‘parameter.in'.

3. Example
In ‘./example/1_Si’ directory, a simulation of the Si atomic deposition process is conducted over two cycles, with a deposition velocity of 0.005 Å/fs. In each cycle, 15 clusters are randomly selected from six distinct clusters configurations (stored in the ‘./clusters’ directory). The deposition substrate is constructed from a-Si, where the atoms in the bottom part are fixed (using the ‘fix’ command in GPUMD), and the upper atoms are coupled to a ‘heat_nhc’ bath to effectively absorb the energy introduced by the deposited atoms. The substrate temperature is maintained at 300K throughout the simulation. The time step is set to 0.5 fs, with 20000 steps per cycle. 
Upon running the script, several initial configurations and essential details will be displayed in the terminal. It can be observed that not all atoms will deposit onto the substrate. This is because some atoms in the clusters move in the opposite direction when the deposition velocity is very low. These atoms are removed based on the ‘h_cutoff’ parameter to prevent interference with subsequent deposition processes.
We note that as additional deposition cycles proceed, distinct growth patterns may emerge. In such cases, it is essential to adjust the x-y range of the deposition atoms according to the specific requirements of the simulation.
![image](https://github.com/user-attachments/assets/2ad5d3c1-f81a-4837-83df-fe16dee3ca09)
