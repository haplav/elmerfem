FC := mpif90
CC := mpicc
MKDIR := mkdir -p
OUTLIBDIR := ../lib

build: test

feti4i_mod.o: ../feti4i_mod.F90
	${FC} -c -fPIC $^

feti4i.mod: 	../feti4i_mod.F90 feti4i_mod.o
	@true

feti4i_dummy.o: feti4i_dummy.c
	${CC} -c -fPIC -g $^

${OUTLIBDIR}/libfeti4i.so: feti4i_dummy.o
	${MKDIR} ${OUTLIBDIR}
	${LINK.F} -o ${OUTLIBDIR}/libfeti4i.so feti4i_dummy.o -shared -fPIC

test.o: test.F90 feti4i.mod
	${FC} -c -fPIC -g test.F90 -o $@

test: test.o ${OUTLIBDIR}/libfeti4i.so
	${LINK.F} -o test test.o -L${OUTLIBDIR} -Wl,-rpath=${OUTLIBDIR} -lfeti4i

clean:
	${RM} test.o feti4i_dummy.o feti4i_mod.o

clean-all: clean
	${RM} feti4i.mod test 
	${RM} -r ${OUTLIBDIR}

.PHONY: build clean

