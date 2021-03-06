# ===========================================================================
#  Makefile for MUSIC                                 Chun Shen Dec. 12, 2014
# ===========================================================================
##
##  Environments :	MAIN	= 	main sourcefile	
##
##  Usage : 	(g)make	[all]		compile the whole project		
##			install	make all and copy binary to $INSTPATH
##			clean		remove objectfiles in obj_$TYPE 
##			distclean	remove all objectsfiles and binaries
##  

CXX := mpiicc
#CXX := CC
#CFLAGS= -Wall -g -O2 -qopenmp $(shell gsl-config --cflags)
# -fsanitize=address -qopt-report=5 -qopt-report-phase:vec
CFLAGS= -g -Wall -xMIC-AVX512 -std=c++11 -O3 -qopenmp -finline-functions -march=knl $(shell gsl-config --cflags) -DDOCTEST_CONFIG_DISABLE -I /opt/cray/pe/fftw/3.3.8.2/mic_knl/include

RM		=	rm -f
O               =       .o
LDFLAGS         =       $(CFLAGS) $(shell gsl-config --libs) -L/opt/cray/pe/fftw/3.3.8.2/mic_knl/lib/ -lfftw3 -lm
SYSTEMFILES     =       $(SRCGNU)

# --------------- Files involved ------------------

ifeq "$(MAIN)" ""
MAIN		=	ipglasma
endif

SRC		=	main.cpp Fragmentation.cpp FFT.cpp Matrix.cpp Setup.cpp Init.cpp Random.cpp Group.cpp Lattice.cpp Cell.cpp Glauber.cpp Util.cpp Evolution.cpp GaugeFix.cpp Spinor.cpp MyEigen.cpp

INC		= 	Fragmentation.h FFT.h Matrix.h Setup.h Init.h Random.h Group.h Lattice.h Cell.h Glauber.h Util.h Evolution.h GaugeFix.h Spinor.h MyEigen.h

# -------------------------------------------------

OBJDIR		=	obj
SRCFILES 	= 	$(SRC) $(INC) GNUmakefile
OBJECTS		=	$(addprefix $(OBJDIR)/, $(addsuffix $O, \
			$(basename $(SRC))))
TARGET		=	$(MAIN)
INSTPATH	=	../

# --------------- Pattern rules -------------------

$(OBJDIR)/%.o: %.cpp
	$(CXX) $(CFLAGS) -c $< -o $@

%.cpp:
	if [ -f $@ ] ; then touch $@ ; else false ; fi

# -------------------------------------------------

.PHONY:		all mkobjdir clean distclean install

all:		mkobjdir $(TARGET)

help:
		@grep '^##' GNUmakefile

mkobjdir:	
		-@mkdir -p $(OBJDIR)

$(TARGET):	$(OBJECTS)	
		$(CXX) $(OBJECTS) -o $(TARGET) $(LDFLAGS) 
#		strip $(TARGET)

clean:		
	rm -f obj/*
	rm -f ipglasma

distclean:	
		-rm $(TARGET)
		-rm -r obj

install:	$(TARGET)
		cp $(TARGET) $(INSTPATH)

# --------------- Dependencies -------------------
