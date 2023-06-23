## MSP430_RS
Files to build docker image with msp430-gcc compiler and rust installed based on ubuntu-latest.

## Build the image
```shell
$ docker compose build msp430_rs
```

## Run the container
```shell
$ docker compose run msp430_rs
```

## Verifying installation
```console
developer@2eebded04864:~$ rustc --version
rustc 1.70.0 (90c541806 2023-05-31)
developer@2eebded04864:~$ msp430-elf-gcc --version
msp430-elf-gcc (Mitto Systems Limited - msp430-gcc 9.3.1.11) 9.3.1
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
