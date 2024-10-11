CC = riscv64-unknown-linux-gnu-gcc
OBJDUMP = riscv64-unknown-linux-gnu-objdump
CFLAGS = -Wall -g -static -march=rv64gv
SRC = $(wildcard ./*.c)
NOTDIR_SRC  = $(notdir $(SRC))
TARGET_LIST = $(patsubst %.c, ./build/%, $(NOTDIR_SRC))
TEST_LIST = $(patsubst %.c, %, $(NOTDIR_SRC))

all: $(TARGET_LIST)

./build/%: ./%.c
	@mkdir -p ./build
	$(CC) $(CFLAGS) -o $@ $<
	$(OBJDUMP) -D $@ > $@.dump

clean:
	-rm -rf ./build ./log

GEM5_ROOT = /data00/home/hongzhibo/share/gem5/gem5
GEM5_CONFIG = $(GEM5_ROOT)/configs/deprecated/example/se.py

test: $(TARGET_LIST)
	@mkdir -p ./log
	@for x in $(TEST_LIST); do \
		$(GEM5_ROOT)/build/RISCV/gem5.opt -e --stdout-file=./log/$$x.log --stderr-file=/dev/null \
			$(GEM5_CONFIG) \
			--cmd ./build/$$x > ./log/$$x.log; \
		# qemu-riscv64 ./build/$$x > ./log/$$x.log; \
		grep pass ./log/$$x.log > /dev/null && echo $$x : pass || echo $$x : fail; \
	done

.PHONY: all clean test $(TEST_LIST)