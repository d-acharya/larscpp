# Makefile for reference implementation of required subset of BLAS
#

##########################################################
# Specify compilers, libraries, flags, and include paths #
##########################################################

LIB_LINKER      = ar
EXE_LINKER      = g++
COMPILER_CPP 	= g++
COMPILER_C 	= gcc
COMPILER_F 	= gcc

# Libraries and library paths
LIBS	 =

# Compiler flags
OPTIM    = -O3 -ffast-math -fexpensive-optimizations
CPPFLAGS = $(INC) -Wno-deprecated $(OPTIM)
CFLAGS	 = $(INC) $(OPTIM)
FFLAGS	 = $(INC) $(OPTIM) -fPIC

# Include paths
INC 	= -I.

#################################
# Specify what files to compile #
#################################

# Source files to compile and archive
FSRC     = daxpy.f ddot.f ddotsub.f dgemv.f dtpsv.f idamax.f idamaxsub.f \
	   isamax.f isamaxsub.f saxpy.f sdot.f sdotsub.f sgemv.f stpsv.f \
	   dgemm.f sgemm.f \
	   xerbla.f lsame.f
CSRC     = 
CPPSRC   = 

################################
# Specify what targets to link #
################################

# Executables and library targets
EXES    = 
LIB     = ../liblarsblas.a

################################
# Specify code file extensions #
################################

# Object files are derived from the source file names this doesn't work!!
CPPOBJS	= $(CPPSRC:.cpp=.o) 
FOBJS   = $(FSRC:.f=.o)
COBJS   = $(CSRC:.c=.o)

# Default option is to link all executables
all: $(EXES) $(LIB)

#######################################
# Specify compiling and linking rules #
#######################################

# Compiler command for CPP
%.o: %.cpp
	@echo "** C++: Compiling '$*' **"
	$(COMPILER_CPP) $(CPPFLAGS) -c $<

# Compiler command for C
%.o: %.c
	@echo "** C: Compiling '$*' **"
	$(COMPILER_C) $(CFLAGS) -c $<

# Compiler command for G77
%.o: %.f
	@echo "** Fortran: Compiling '$*' **"
	$(COMPILER_F) $(FFLAGS) -c $<

# Execuatable Linker command
$(EXES): $(EXES:=.o) ${CPPOBJS} $(COBJS) $(FOBJS)
	@echo "  "
	@echo "** Linking '$@' **"
	$(EXE_LINKER) $(CPPOBJS) $(COBJS) $(FOBJS) $(@:=.o) $(LIBS) -o $@

# Library linker command
$(LIB): $(CPPOBJS) $(COBJS) $(FOBJS)
	@echo "  "
	@echo "** Linking library '$@' **"
	$(LIB_LINKER) rcs $@ $(CPPOBJS) $(COBJS) $(FOBJS)


#########################
# Clean and depend code #
#########################

# Delete all object files
clean:
	rm *.o

# Determine make dependencies
depend makedep: $(CPPSRC) $(FSRC) $(EXES:=.cpp)
	@echo making depend
	$(SHELL) -ec 'g++ -MM $^' > make.dep
	@echo done dependency list

# Include dependencies, as generated by 'make depend'
sinclude make.dep
# DO NOT DELETE
