UNAME := $(shell uname)

CC=gcc
CFLAGS= -Wall -Wextra -Werror -std=c11 -pedantic

SOURCES=s21_*.c
OBJECTS=s21_*.o


all: s21_math.a test

s21_math.a:
	$(CC) $(CFLAGS) -c $(SOURCES)
	ar rc s21_math.a $(OBJECTS)
	ranlib s21_math.a
	rm -f *.o
	
ifeq ($(UNAME), Linux)
test: s21_math.a
	$(CC) $(CFLAGS) -c test/test_math.c
	ar rc cases.a test_math.o
	ranlib cases.a
	$(CC) $(CFLAGS) --coverage -o c_tests s21_math.a cases.a $(SOURCES) -lcheck -lm -lsubunit -lrt -lpthread
	./c_tests
else
test: s21_math.a
	$(CC) $(CFLAGS) -c test/test_math.c
	ar rc cases.a test_math.o
	ranlib cases.a
	$(CC) $(CFLAGS) --coverage -o c_tests s21_math.a cases.a $(SOURCES) -lcheck -lm -pthread
	./c_tests
endif

gcov_report:
	gcovr -r . --html --html-details -o coverage_report.html
	./c_tests

cppcheck: 
	cppcheck --enable=all --suppress=missingIncludeSystem *.c *.h
linter:
	python3 ../materials/linters/cpplint.py --extensions=c *.c *.h
check_all: cppcheck linter

push: 
	git add .
	git commit -m "insertion"
	git push origin fursthug

clean:
	rm *.a *.html *.css *.gcno *.gcda *.o ./c_tests
