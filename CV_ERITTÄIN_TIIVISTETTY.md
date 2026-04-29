# Binary Fusion - CV Tiivistelmä (Erittäin Tiivis)

## Teknologiat
**Kielet:** C++17, C, Bash  
**Build:** Makefile, GCC toolchain (gcc, g++, ld, objcopy, strip)  
**Työkalut:** readelf, objdump, ELF-parsinta  
**Järjestelmä:** Linux, fork/exec, mmap, static linking

## Opetut Taidot
- ELF-binäärien parsinta ja manipulaatio
- Binääri-embedding ja osioiden yhdistäminen
- Linker-työkalun käyttö symbolien resoluutioon
- Järjestelmäohjelmointi (prosessien hallinta)
- Build-automaatio ja testaus

## Projektin Tulos
Työkalu, joka yhdistää kaksi ELF-suoritettavaa tiedostoa yhdeksi binääriksi. Toteuttaa kaksi eri lähestymistapaa: binääri-embedding (fork/exec) ja todellinen osioiden yhdistäminen (linker).

