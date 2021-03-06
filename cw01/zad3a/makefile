CC = gcc
FLAGS = -std=c99 -Wall
LIB = lib
MAIN = main
REPORT_FILE = "raport3a.txt"

CROSS = create_table 100000000 add_delete_loop 10000000
SHALLOW = create_table 100000 search_directory . makefile temp.txt dump remove_block 0
DEEP_BIG = create_table 100000 search_directory /usr \'*.txt\' temp.txt dump remove_block 0
DEEP_HUGE = create_table 10 search_directory /usr \'*\' temp.txt dump remove_block 0

all: static shared dynamic measurements clean

static:
	$(CC) $(FLAGS) -c $(LIB).c -o $(LIB).o
	ar rcs $(LIB).a $(LIB).o
	$(CC) $(FLAGS) $(MAIN).c -I. $(LIB).a -o $(MAIN)_static
	make clean

shared:
	$(CC) -fPIC $(FLAGS) -c $(LIB).c -o $(LIB).o
	$(CC) -fPIC $(FLAGS) -shared -o $(LIB).so $(LIB).o
	$(CC) $(FLAGS) -Wl,-rpath=. -L. $(LIB).so -o $(MAIN)_shared $(MAIN).c
	make clean

dynamic:
	$(CC) -fPIC $(FLAGS) -c $(LIB).c -o $(LIB).o
	$(CC) -fPIC $(FLAGS) -shared -o $(LIB).so $(LIB).o
	$(CC) $(FLAGS) -Wl,-rpath=. -L. -o $(MAIN)_dynamic $(MAIN).c -ldl -D DLL
	make clean

clean:
	rm -f *.o
	rm -f *.a

purge: clean
	rm -f *.so
	rm -f $(MAIN)_shared
	rm -f $(MAIN)_static
	rm -f $(MAIN)_dynamic

measurements:
	printf "STATIC\n\n" >> $(REPORT_FILE)
	./$(MAIN)_static $(CROSS) | tee -a $(REPORT_FILE)
	./$(MAIN)_static $(SHALLOW) | tee -a $(REPORT_FILE)
	./$(MAIN)_static $(DEEP_BIG) | tee -a $(REPORT_FILE)
	./$(MAIN)_static $(DEEP_HUGE) | tee -a $(REPORT_FILE)
	printf "SHARED\n\n" >> $(REPORT_FILE)
	./$(MAIN)_shared $(CROSS) | tee -a $(REPORT_FILE)
	./$(MAIN)_shared $(SHALLOW) | tee -a $(REPORT_FILE)
	./$(MAIN)_shared $(DEEP_BIG) | tee -a $(REPORT_FILE)
	./$(MAIN)_shared $(DEEP_HUGE) | tee -a $(REPORT_FILE)
	printf "DYNAMIC\n\n" >> $(REPORT_FILE)
	./$(MAIN)_dynamic $(CROSS) | tee -a $(REPORT_FILE)
	./$(MAIN)_dynamic $(SHALLOW) | tee -a $(REPORT_FILE)
	./$(MAIN)_dynamic $(DEEP_BIG) | tee -a $(REPORT_FILE)
	./$(MAIN)_dynamic $(DEEP_HUGE) | tee -a $(REPORT_FILE)