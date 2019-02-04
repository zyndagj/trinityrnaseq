
OS := $(shell uname)

CXX ?= g++
CC ?= gcc
COLLECTL_CODE="collectl-4.1.0"
PARAFLY_CODE="ParaFly-0.1.0"
TRIMMOMATIC_CODE=Trimmomatic-0.36



trinity_essentials: seqtk_target parafly_target trimmomatic_target
	rm -rf scaffold_iworm_contigs

trimmomatic_target:
	mv ${TRIMMOMATIC_CODE} Trimmomatic

seqtk_target:
	cd seqtk-trinity-0.0.2 && $(MAKE) CXX=$(CXX) CC=$(CC)
	mv seqtk-trinity-0.0.2/seqtk-trinity ./BIN/.
	rm -rf seqtk-trinity*

parafly_target:
	tar -zxf ${PARAFLY_CODE}.tar.gz && \
	cd ${PARAFLY_CODE} && sed -i "s/\-openmp/\-qopenmp/" Make* config*
	cd ${PARAFLY_CODE} && sh ./configure --prefix=`pwd` CXX=$(CXX) CC=$(CC) CFLAGS="-qopenmp" CXXFLAGS="-qopenmp" && $(MAKE) install && \
	cp bin/ParaFly ../BIN/
	rm -rf ${PARAFLY_CODE}*

##################################
### Needed for downstream analyses
##################################

plugins: slclust_target collectl_target
	@echo "\n\n** Done building plugins **\n\n"

slclust_target:
	cd slclust && $(MAKE) CXX=$(CXX) CC=$(CC) CXXFLAGS="$(CFLAGS) -I../include -fast" install
	mv slclust/bin/* BIN/
	rm -rf slclust/*
	mkdir slclust/bin
	mv BIN/slclust slclust/bin

collectl_target:
	cd COLLECTL && tar xvf ${COLLECTL_CODE}.src.tar.gz \
	&& rm ${COLLECTL_CODE}.src.tar.gz \
	&& mv ${COLLECTL_CODE} collectl \
	&& rm -rf collectl/docs

clean:
	cd seqtk-trinity-0.0.2 && $(MAKE) clean
	cd scaffold_iworm_contigs && $(MAKE) clean
	rm -rf ${PARAFLY_CODE}
	rm -f ./Trimmomatic # rm symlink
	cd slclust && $(MAKE) clean
	cd COLLECTL && rm -rf ${COLLECTL_CODE} && rm -f collectl
	@echo "\n\n** Done cleaning plugins area **"


###################################################################
