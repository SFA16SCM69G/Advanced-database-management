#The contants cc will communicate with "make" ,the macro CC is the C compiler to use
CC = gcc
# execute the make command, the default execute command is the "test". Because it is the first one.
# test command will depend on the test_assign2_1.o storage_mgr.o dberror.o and put them in the ./test
test: test_assign2_1.o storage_mgr.o dberror.o buffer_mgr.o buffer_mgr_stat.o
	$(CC) -o test test_assign2_1.o storage_mgr.o dberror.o buffer_mgr.o buffer_mgr_stat.o

#make test_assign2_1.o, will build the test_assign2_1.c and produce the test_assign2_1.o 
test_assign2_1.o: test_assign2_1.c dberror.h storage_mgr.h test_helper.h buffer_mgr.h buffer_mgr_stat.h
	$(CC)  -c test_assign2_1.c 

# make storage_mgr.o will gcc buffer_mgr_stat.c
buffer_mgr_stat.o: buffer_mgr_stat.c buffer_mgr_stat.h buffer_mgr.h
	$(CC)  -c buffer_mgr_stat.c

# make storage_mgr.o will gcc buffer_mgr.c
buffer_mgr.o: buffer_mgr.c buffer_mgr.h dt.h storage_mgr.h
	$(CC) -c buffer_mgr.c

# make storage_mgr.o will gcc storage_mgr.c
storage_mgr.o: storage_mgr.c storage_mgr.h 
	$(CC)  -c storage_mgr.c 
#gcc dberror.c
dberror.o: dberror.c dberror.h 
	$(CC)  -c dberror.c

#Use make clean to delete all files produced by the make command.Recovered to the beginning status.
clean: 
	$(RM)  test *.o *~ *.log 

# make test_run command will execute the result by running the testcase1.
test_run: 
	./test