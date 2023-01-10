CC = gcc
FLAGS = -Wall -Wextra -Werror -std=c11
TST_LIBS = -lcheck
ifeq ($(shell uname), Linux)
	TST_LIBS += -lpthread -lrt -lsubunit -lm
endif
SRCS=$(wildcard s21_*.c)
OBJS=$(SRCS:.c=.o)

LIB_NAME = s21_matrix
TEST_FILES = $(wildcard test*.c)

all: clean test

clean:
	rm -rf *.o test *.a *.gcno *.gcda *.gcov *.html *.css gcov_report/ coverage.info

%.o: %.c
	$(CC) $(FLAGS) -c $< -o $@

$(LIB_NAME).a: $(OBJS)
	ar rc $(LIB_NAME).a $^
	ranlib $(LIB_NAME).a
	rm -rf *.o

linter:
	python3 ../cpplint.py --extensions=c *.c *.h # tests/*.c tests/*.h

clang-format:
	clang-format -style=Google -n $(wildcard *.c)
	clang-format -style=Google -i $(wildcard *.c)

# check - либа для работы тестов, $(LIB_NAME - наша либа
test: clean $(LIB_NAME).a
	$(CC) $(FLAGS) $(TEST_FILES) -L. $(LIB_NAME).a $(TST_LIBS) -o test
	./test

main: $(LIB_NAME).a
	$(CC) $(FLAGS) main.c $(LIB_NAME).a $(TST_LIBS) -o Main
	./Main

add_coverage_flag:
	$(eval FLAGS += --coverage)

gcov_report: add_coverage_flag test
#	gcov -b -l -p -c s21_*.gcno
#	gcovr -o gcov_report.html --html --html-details

	rm test*.gcda test*.gcno
	lcov --capture --directory . --output-file coverage.info
	genhtml coverage.info --output-directory gcov_report
