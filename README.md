# LAMMPS_Heat_Transfer_Package
An algorithm implementation in LAMMPS for surface to liquid heat transfer at constant temperature


Author:  Manish Gupta (Syracuse University ) 
Advisor: Dr. Shalabh C. Maroo (Syracuse University)
Ackowledgement: Shishir Bijalwan (Syracuse University)




for Ar heating on platinum and water heating on platinum - please place these files in src folder and build lammps again


file names- 

1. atom.cpp   			//replace
2. atom.h			//replace
3. atom_vec_heat_full.cpp
4. atom_vec_heat_full.h
5. fix_nhheat.cpp
6. fix_nhheat.h
7. fix_nvtheat.cpp
8. fix_nvtheat.h
9. pair_lj_smoothheat.cpp
10. pair_lj_smoothheat.h

//place these two files in Kspace subdirectory of src
11. pair_lj_cut_coul_longheat.cpp	// for water heating
12. pair_lj_cut_coul_longheat.h 	//for water heating with kpsace pppm


After building lammps again with Kspace module - 

1. atom_style heatfull

for argon heating
2. pair_style lj/smoothheat rcut1 rcut2 EquilibriumConstant 
// where rcut1 and rcut2 are the cutoff distance for LJ and smoothness .. 
// EqulibriumConstant is the number when you want to "switch on" heating. 
// When runstep/timestep < EquilibriumConstant -- would work normally as if thermostat is used- heating will not happen
// When timestep > EquilibriumConstant - Heating would happen


for water heating
2. pair_style lj/cut/coul/longheat rcut1 rcut2 EquilibriumConstant 
// where rcut1 Lj cuttoff
// rcut2 coulombian cuttoff .. 
// EqulibriumConstant is the number when you want to "switch on" heating. 
// When runstep/timestep < EquilibriumConstant -- would work normally as if thermostat is used- heating will not happen
// When timestep > EquilibriumConstant - Heating would happen


3. fix fixID groupID nvtheat temp Tstart Tstop Tdamp fluidtype regionID1 T1 Dir1 regionID2 T2 Dir2 ....

//If timestep < equilibriumconstant -- NVT thermostat would work 
// fixID - user defined name 
// groupID - should contain the group of liquid needed to be heated
// nvtheat - style_name
// temp - temperature NH thermostat
// Tstart, Tstop -- external temperature at start/end of run when thermostat is working
// Tdamp -- temperature damping parameter (temperature unit) -- when thermostat is working 
// fluidtype -- either argon or water -- depending on the polar or non-polar -- Heating atoms should always be type 1 ( oxygen or     argon)
// RegionID -- name of the 3D region defined which needs to be heated -- can only work on block type of region 
// T1 - Temperature of the desired surface 
// Dir1 - Normal of the heated surface 
// Multiple number of regions can be added in the fix 
// Example 
	region heat1 block 0 100 0 3.552 0 50 
	region heat2 block 0 100 5 50 0 3.552 
	fix fixnvtheat argon nvtheat temp 110 110 100 argon heat1 120 y heat2 140 z
 //region block will define the critical distance of heating algorithm. 



