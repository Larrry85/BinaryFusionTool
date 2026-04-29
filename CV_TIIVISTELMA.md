# Binary Fusion - CV Tiivistelmä

## Projektin Kuvaus
Binary Fusion -työkalu, joka yhdistää kaksi ELF-suoritettavaa tiedostoa yhdeksi binääriksi. Projekti toteuttaa kaksi eri lähestymistapaa binäärien yhdistämiseen: binääri-embedding ja todellinen osioiden yhdistäminen.

## Teknologiat & Työkalut

### Ohjelmointikielet
- **C++17** - Pääohjelmointikieli (ELF-parsinta, binäärimanipulaatio)
- **C** - Järjestelmäkutsut, fork/exec, muistinhallinta
- **Bash/Shell** - Automaatiot, testausskriptit

### Build & Development
- **Makefile** - Build-järjestelmä, riippuvuuksien hallinta
- **GCC Toolchain** - gcc, g++, ld (linker), objcopy, strip
- **Linux** - ELF-tuki, järjestelmäohjelmointi

### Binäärianalyysi & Manipulaatio
- **ELF-format** - Header-parsinta, osioiden käsittely
- **readelf, objdump** - Binäärianalyysi
- **objcopy** - Symbolien uudelleennimeäminen, binäärien muuntaminen

### Järjestelmäohjelmointi
- **fork/exec** - Prosessien hallinta
- **mmap** - Muistinmappaus
- **Static linking** - Staattinen linkitys

## Opetut Taidot

### Tekniset Taidot
- ELF-binäärien parsinta ja validointi
- Binäärimanipulaatio ja osioiden yhdistäminen
- Linker-työkalun käyttö symbolien resoluutioon
- Järjestelmäohjelmointi (fork, exec, waitpid)
- C++ STL (vektorit, mapit, algoritmit)
- Build-automaatio Makefilella
- Testausautomaatio Bash-skripteillä

### Käsitteet
- ELF-format (ET_EXEC, ET_REL, ET_DYN)
- Staattinen vs. dynaaminen linkitys
- Symbolien resoluutio ja uudelleennimeäminen
- Binääri-embedding vs. todellinen yhdistäminen
- Prosessien hallinta ja fork/exec-mallit

## Projektin Ominaisuudet
- Kaksi eri toteutustapaa (embedding & concatenation)
- Automaattinen validointi ja virheenkäsittely
- Kattava testausautomaatio
- Dokumentaatio prosesseista

