vpath %.h ../../src/testsort ../../../util
vpath %.cpp ../../src/testsort ../../../util ../../src/cudpp2
vpath %.cu ../../src/testsort ../../src/cudpp2

INC = cucpp.h util.h benchmark.h 
SRC = cucpp.cpp mgputest.cpp testsort.cpp cudppsort.cpp cudpp.cpp \
	cudpp_manager.cpp cudpp_maximal_launch.cpp cudpp_plan.cpp
CU = b40ctest.cu thrusttest.cu 

.PHONY: release
.PHONY: debug

release: CPPFLAGS1 = -Wall -I /usr/local/cuda/include -c -O3
release: CUFLAGS = -I /usr/local/cuda/include -c -O3 -arch=compute_20 -code=sm_20
release: CPPFLAGS2 = -Wall -L /usr/local/cuda/lib -lcuda -lcudart
debug: CPPFLAGS1 = -Wall -I /usr/local/cuda/include -c -g -O0
debug: CUFLAGS = -I /usr/local/cuda/include -c -O0 -g -arch=compute_20 -code=sm_20
debug: CPPFLAGS2 = -Wall -L /usr/local/cuda/lib -lcuda -lcudart

release: ../release/testsort
debug: ../debug/testsort

../release/testsort : release/testsort.o release/cucpp.o release/mgputest.o  \
	release/cudppsort.o release/cudpp.o release/cudpp_manager.o \
	release/cudpp_maximal_launch.o release/cudpp_plan.o \
	release/thrusttest.cuo release/b40ctest.cuo	release/radixsort_app.cuo \
	release/scan_app.cuo
	@mkdir -p ../release
	g++ $(CPPFLAGS2) -o $@ $^ ../release/libmgpusort.a

release/%.o : %.cpp $(INC)
	@mkdir -p release
	g++ $(CPPFLAGS1) -o $@ $<

release/%.cuo : %.cu $(INC)
	@mkdir -p release
	nvcc $(CUFLAGS) -I ../../src/b40c/ -o $@ $<


../debug/testsort : debug/testsort.o debug/cucpp.o debug/mgputest.o  \
	debug/cudppsort.o debug/cudpp.o debug/cudpp_manager.o \
	debug/cudpp_maximal_launch.o debug/cudpp_plan.o \
	debug/thrusttest.cuo debug/b40ctest.cuo	debug/radixsort_app.cuo \
	debug/scan_app.cuo
	@mkdir -p ../debug
	g++ $(CPPFLAGS2) -o $@ $^ ../debug/libmgpusort.a

debug/%.o : %.cpp $(INC)
	@mkdir -p debug
	g++ $(CPPFLAGS1) -o $@ $<

debug/%.cuo : %.cu $(INC)
	@mkdir -p debug
	nvcc $(CUFLAGS) -I ../../src/b40c/ -o $@ $<

.PHONY: clean
clean:
	@-rm debug/*.o
	@-rm debug/*.cuo
	@-rm ../debug/testsort
	@-rm release/*.o
	@-rm release/*.cuo
	@-rm ../release/testsort
