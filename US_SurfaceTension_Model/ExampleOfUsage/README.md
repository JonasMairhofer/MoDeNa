@ingroup app_foaming


#Example simulation with Surface Tension model:

##Description
A simple macroscopic code calls the Surface Tension model for different temperatures.


##How to run?

###Make sure PYTHONPATH and LD_LIBRARY_PATH are set:
export PKG_CONFIG_PATH=${PKG_CONFIG_PATH:-}:${HOME}/lib/pkgconfig:/usr/local/lib/pkgconfig
export PYTHONPATH=${PYTHONPATH:-}:${HOME}/lib/python2.7/site-packages
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH:-}:${HOME}/lib/python2.7/site-packages:${HOME}/lib:/usr/local/lib

### Compile macroscopic code
cd src
cmake .
make


### Compile detailed model code (PETSc 3.4.4 needs to be installed)
cd src/srcDetailedCode/
make DFT

###copy executable of detailed model (PCSAFT_Henry) in src folder


### Initialise the model in the database
./initModel

### Start the workflow
./workflow
