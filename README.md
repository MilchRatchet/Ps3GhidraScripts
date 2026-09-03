
# Ps3GhidraScripts
A collection of scripts for parsing PS3 executables with Ghidra.

Relocations are not currently supported.

When loading a prx/elf into Ghidra be sure to select the following language (by default this is the one under the automatic selection when the recommended checkbox is disabled)
```
PowerISA-Altivec-64-32addr with PS3 PPU instructions
```
Make sure to select the BIG endian, otherwise the scripts will throw an error upon running them.

The PS3 language variant adds Cell-specific PPU instructions such as `lvlx`. For example,
the big-endian bytes `7C C4 3C 0E` disassemble as `lvlx v6, r4, r7`.

## Installation

These scripts are meant to be used as a Ghidra extension.

Simply grab the .zip in release corresponding to your Ghidra version and install in Ghidra through "File=>Install Extension...".

Make sure the extension is active(there should be a checkmark on the left), scripts should then be accessible in CodeBrowser through "Window=>Script Manager".

## Compiler specification
The extension automatically includes the `r2` register in the PS3 language's
`<unaffected>` list to avoid decompilation issues. The original PowerPC compiler
specification in the Ghidra installation is not modified.

## Possible problems
Some Cell-specific instructions other than those implemented by the PS3 language variant
are currently not supported in Ghidra. These may appear in games and may break decompilation.

## AnalyzePs3Binary.java
The main script, this should be used BEFORE analysis is run on the program.

It will then parse the information sections and define imports/exports and name the ones it can from the nids file, and then set the TOC.

After this you should run the auto analysis tool within Ghidra, and then run the syscall define script.

## DefinePs3Syscalls.java
Does what it says on the tin.

Resolves PS3 syscalls to the correct name and defines functions for them.

Should be run after AnalyzePs3Binary and auto analysis have completed.
