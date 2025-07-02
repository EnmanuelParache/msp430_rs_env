FROM ubuntu:20.04
ARG USERNAME=developer
ARG UID=1001
ARG MSP430_GCC=msp430-gcc-full-linux-x64-installer-9.3.1.2
ARG MSP430_GCC_COMPRESSED=${MSP430_GCC}.7z
ARG TI_MSP430_DRIVER=ti_msp430driver_setup_1.0.1.2
ARG TI_MSP430_DRIVER_COMPRESSED=${TI_MSP430_DRIVER}-linux64.zip
ARG TARGET=msp430-none-elf
ARG TI_INSTALL_DIR=/home/${USERNAME}/.ti/

# Use bash as shell
SHELL ["/bin/bash", "-c"]

# Update 
RUN apt-get update -y

# Upgrade
RUN apt-get upgrade -y

# Install dependencies
RUN apt-get install -y libc6-i386 libusb-0.1-4 udev unzip curl build-essential sudo git p7zip-full unzip mspdebug

# Set home as working directory
WORKDIR /home

# Create user
RUN adduser --disabled-password --system --shell /bin/bash --uid ${UID} ${USERNAME}

# Create group for usb
RUN addgroup usb

# Add user to usb group
RUN adduser ${USERNAME} usb

# Add user to sudo group
RUN adduser ${USERNAME} sudo

# Add user to sudoers to not require password
RUN echo "${USERNAME} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${USERNAME}

# Change user
USER ${USERNAME}

# Set user home as working directory
WORKDIR /home/${USERNAME}

# Create ti install dir
RUN mkdir ${TI_INSTALL_DIR}

# Copy msp430-gcc installer
COPY ${MSP430_GCC_COMPRESSED} .

# Extract msp430-gcc installer
RUN 7z x ${MSP430_GCC_COMPRESSED}

# Remove msp430-gcc compressed
RUN rm ${MSP430_GCC_COMPRESSED}

# Install msp430-gcc
RUN sudo ./${MSP430_GCC}.run --mode unattended --prefix ${TI_INSTALL_DIR}/msp430-gcc

# Remove msp430-gcc installer
RUN rm ${MSP430_GCC}.run

# Copy ti_msp430driver
COPY ${TI_MSP430_DRIVER_COMPRESSED} .

# Extract ti_msp430driver
RUN unzip ${TI_MSP430_DRIVER_COMPRESSED}

# Remove ti_msp430driver
RUN rm ${TI_MSP430_DRIVER_COMPRESSED}

# Install ti_msp430driver
RUN sudo ./${TI_MSP430_DRIVER}.bin --mode unattended

# Remove ti_msp430_driver installer binaries
RUN rm ${TI_MSP430_DRIVER}.bin

# Install rustup
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --target ${TARGET}

# Create bash profile including msp430-gcc
RUN echo "export PATH=$PATH:$HOME/.ti/msp430-gcc/bin:$HOME/.cargo/bin" > .bashrc

# Install nightly 
RUN .cargo/bin/rustup install nightly

# Create symbolic link for libmsp430.so
RUN sudo ln -sf /home/developer/.ti/msp430-gcc/bin/libmsp430.so /usr/lib/libmsp430.so

# Set nightly 
# RUN .cargo/bin/rustup default nightly

# Set rust-src
RUN .cargo/bin/rustup +nightly component add rust-src

# Add msp430-none-elf
# RUN rustup target add msp430-none-elf

# Run bash
CMD ["/bin/bash"]