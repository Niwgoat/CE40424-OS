TARGET_MODULE := custom_opener
ccflags-y := -std=gnu99
obj-m := $(TARGET_MODULE).o


KDIR := /lib/modules/$(shell uname -r)/build
PWD := $(shell pwd)

all: user
	$(MAKE) -C $(KDIR) M=$(PWD) modules


clean:
	$(MAKE) -C $(KDIR) M=$(PWD) clean
	$(RM) user out

load:
	sudo insmod ./$(TARGET_MODULE).ko
unload:
	sudo rmmod $(TARGET_MODULE) || true >/dev/null

client: user.c
	$(CC) -o $@ $^

PRINTF = env printf
PASS_COLOR = \e[32;01m
NO_COLOR = \e[0m
pass = $(PRINTF) "$(PASS_COLOR)$1 Passed [-]$(NO_COLOR)\n"

check: all
	$(MAKE) unload
	$(MAKE) load
	sudo ./client > out
	$(MAKE) unload
