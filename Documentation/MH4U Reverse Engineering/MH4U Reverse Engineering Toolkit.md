
# Getting Started

This section will cover the steps taken to reverse engineer Monster Hunter 4 Ultimate.

## Setting up the Toolchain

To follow this guide you'll have to clone this [repository](https://gitlab.com/arthur_muraro/mh4uonline.git). This repo will contain a modified version of [ctr-elf](https://github.com/archshift/ctr-elf) and a copy of [3dstool](https://github.com/dnasdw/3dstool).  You will have to build 3dstool locally on your machine, follow the instructions provided in the repository. If you own an Apple Silicon Macbook you might find this helpful [[Building 3dstool on Apple Silicon]].

The main idea of idea is to transform .3ds files into ELF format. This section will cover the process of doing that. As a rough layout, the process can be visualized as such:

```plaintext
.3ds -> .cxi -> ARM11 code -> ELF
```


You can find more information about the [[Nintendo 3DS File Formats]]


**Extract the .3ds files**

```bash
./tools/3dstool/bin/Release/3dstool -xvt017f cci cci_extract/0.cxi cci_extract/1.cfa cci_extract/7.cfa <PATH_TO_YOUR_ROM> --header cci_extract/ncsdheader.bin
```

**Extract the .cxi files**

```bash
./tools/3dstool/bin/Release/3dstool -xvtf cxi cci_extract/0.cxi --header cci_extract/ncchheader.bin --exh cci_extract/exh.bin --logo cci_extract/logo.bcma.lz --plain cci_extract/plain.bin --exefs cci_extract/exefs.bin --romfs cci_extract/romfs.bin
```

**Extracting ExeFS**

```bash
./tools/3dstool/bin/Release/3dstool -xtf exefs cci_extract/exefs.bin --exefs-dir cci_extract/exefs/
```

**Converting ExeFS to ELF**

```bash
python3 tools/ctr-elf2_custom/make_elf.py cci_extract
```


## Using Ghidra for Analysis

With the completion of the previous steps, you should find the ```ExeFs.elf``` file within your ```cci_extract``` directory. Proceed to open this file using a preferred analysis tool. For the purpose of this demonstration, we will utilize Ghidra, an open-source software suite available at [this repository](https://github.com/NationalSecurityAgency/ghidra). Through Ghidra, you can begin the analysis of the file to gain insights into its structure and functionality.