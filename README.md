# day0-installation
Get access and how to compile RSPt.

To obtain the RSPt software one needs to email the administrators before downloading the code from github.
Please find information about this [here](http://www.physics.uu.se/research/materials-theory/ongoing-research/code-development/rspt-main/faq-on-rspt/). 

Once granted access to the repository, move in the terminal to your desired location for RSPt.
E.g. to login to Uppmax's supercomputer Rackham, type:
```
ssh -X your_user_name@rackham.uppmax.uu.se
```
Before downloading the repository from github it can be a good idea to make sure you have a decently new version of `git` installed.
E.g. on Rackham, type: 
```
module load git
```
To download RSPt from github type:
```
git clone https://github.com/uumaterialstheory/rspt.git
```
Compilation of RSPt can be done for serial or parallel usage.
On a supercomputer one typically needs to load some modules.
E.g. on Rackham, load needed modules by typing:
```
module load intel/18.1 intelmpi/18.1
```
A `RSPTmake.inc` is needed in the parent directory for compilation.
Examples for different machines exist in the folder `RSPTmakes`.
For an MPI compilation on Rackham, `RSPTmake.inc` can contain the following:
```
## Compilers
# Intel compilers on Rackham
# module load intel/18.1 intelmpi/18.1

OPT_FLAGS = -O2
#OPT_FLAGS += -g
DEFINITIONS = -DMEMORY_STORE
#DEFINITIONS += -DMEMORY_STORE -DMPI_FFTW -DMKLFFT

FCOMPILER        = mpiifort
FCOMPILERFLAGS   = $(OPT_FLAGS) -assume underscore
FCPPFLAGS        = -DF_APPENDS_UNDERSCORE=0 -DMPI $(DEFINITIONS)
FTARGETARCH      =
FORTRANLIBS      = -lifcore -lsvml -lirc -lifport -limf
F90COMPILER      = mpiifort
F90COMPILERFLAGS = -cpp -extend_source -assume underscore $(OPT_FLAGS)
# icc
CCOMPILER        = mpiicc
CPPCOMPILER      = mpiicc
CCOMPILERFLAGS   = $(OPT_FLAGS) -fno-gnu-keywords
CTARGETARCH      =
CPPFLAGS         = -DF_APPENDS_UNDERSCORE=0 -DMPI $(DEFINITIONS)
CLOADER          =

# Flag that MKL FFT wrappers should be generated
#MKL_FFTW_WRAPPER = intel intelmpi

## LIBRARIES AND INCLUDE DIRECTORIES
LAPACKLIB        = $$MKL_LDFLAGS -lmkl_intel_lp64 -lmkl_core -lmkl_sequential -lpthread -lm
INCLUDEDIRS      = -I$$MKLROOT/include -I$$MKLROOT/include/fftw
```
Compile by typing:
```
make 
```
To have RSPt's binaries accessable from any folder it can be useful to create environment variables:
```
export RSPTHOME=path/to/RSPt/folder
export PATH=$PATH:${RSPTHOME}/bin
```
