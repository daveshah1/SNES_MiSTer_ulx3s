FPGA_SIZE ?= 85

YOSYS_GHDL_PLUGIN="-m $(PLUGIN_DIR)/ghdl.so"

FPGA_KS ?= $(FPGA_SIZE)k

ifeq ($(FPGA_SIZE), 12)
	CHIP_ID=0x21111043
endif
ifeq ($(FPGA_SIZE), 25)
	CHIP_ID=0x41111043
endif
ifeq ($(FPGA_SIZE), 45)
	CHIP_ID=0x41112043
endif
ifeq ($(FPGA_SIZE), 85)
	CHIP_ID=0x41113043
endif

IDCODE ?= $(CHIP_ID)

all: snes.bit

prog: snes.bit
	fujprog $<

snes.json: 
	yosys -q $(YOSYS_GHDL_PLUGIN) synth.ys

snes.config: snes.json
	nextpnr-ecp5 --json snes.json --$(FPGA_KS) --speed 8 --package CABGA381 --textcfg snes.config --lpf ../top/ulx3s_v20.lpf --timing-allow-fail --no-tmdriv

snes.bit: snes.config
	ecppack --compress --idcode $(IDCODE) $< $@

clean:
	$(RM) -rf snes.bit snes.config snes.json
