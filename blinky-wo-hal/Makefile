#CROSS_COMPILE=arm-none-eabi-
CROSS_COMPILE =/opt/gnu/gcc-arm-none-eabi-7-2017-q4-major/bin/arm-none-eabi-
CC=$(CROSS_COMPILE)gcc
LD=$(CROSS_COMPILE)ld
AR=$(CROSS_COMPILE)ar
AS=$(CROSS_COMPILE)as
OC=$(CROSS_COMPILE)objcopy
OD=$(CROSS_COMPILE)objdump
SZ=$(CROSS_COMPILE)size

CFLAGS= -c -fno-common \
	-ffunction-sections \
	-fdata-sections \
	-Os \
	-g3 \
	-mcpu=cortex-m3 -Wall \
	-mthumb -mfloat-abi=soft

LDSCRIPT=ld/stm32_rom.ld
LDFLAGS	= --gc-sections,-T$(LDSCRIPT),-no-startup,-nostdlib,-lnosys
OCFLAGS	= -Obinary
ODFLAGS	= -S
BUILD_DIR = build
TARGET  = $(BUILD_DIR)/blinky-wo-hal

INCLUDE = -I./src/fw_lib/include \
	  -I./src/include \

SRCS = 	./src/system_stm32f10x.c \
	./src/stm32f10x_it.c \
	./src/newlib_stubs.c \
	./src/fw_lib/core_cm3.c \
	./src/fw_lib/stm32f10x_rcc.c \
	./src/fw_lib/stm32f10x_gpio.c \
	./src/fw_lib/misc.c \
	./src/main.c 


OBJS=$(SRCS:.c=.o)

.PHONY : clean all

all: $(TARGET).bin $(TARGET).hex $(TARGET).list   
	$(SZ) $(TARGET).elf

clean:
	-find . -name '*.o'   -exec rm {} \;
	-find . -name '*.elf' -exec rm {} \;
	-find . -name '*.lst' -exec rm {} \;
	-find . -name '*.out' -exec rm {} \;
	-find . -name '*.bin' -exec rm {} \;
	-find . -name '*.map' -exec rm {} \;

$(TARGET).list: $(TARGET).elf
	$(OD) $(ODFLAGS) $< > $(TARGET).lst

$(TARGET).hex: $(TARGET).elf
	$(OC) -O ihex $(TARGET).elf $(TARGET).hex

$(TARGET).bin: $(TARGET).elf
	$(OC) $(OCFLAGS) $(TARGET).elf $(TARGET).bin

$(TARGET).elf: $(OBJS) ./src/startup.o
	@$(CC) -mcpu=cortex-m3 -mthumb -Wl,$(LDFLAGS),-o$(TARGET).elf,-Map,$(TARGET).map ./src/startup.o $(OBJS)

%.o: %.c
	@echo "  CC $<"
	@$(CC) $(INCLUDE) $(CFLAGS)  $< -o $*.o

%.o: %.S
	@echo "  CC $<"
	@$(CC) $(INCLUDE) $(CFLAGS)  $< -o $*.o
