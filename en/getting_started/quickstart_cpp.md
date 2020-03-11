# QuickStart

This quickstart hows how to install the core MAVSDK C++ Core library on all supported platforms.

For macOS and Ubuntu Linux it also shows how to run a simple example against a simulated vehicle, and observe the operation using the console and in a ground station.

> **Note** To get started using other programming languages see:
  - [Python QuickStart] 
  - [Swift QuickStart]

## Installation

This section show how to install the MAVSDK core library on various platforms.
  
- **MacOS:** Install [Homebrew](https://brew.sh/) and use it to install the library:
  ```
  brew install mavsdk
  ```
- **Ubuntu:** Download the **.deb** file for your system from [MAVSDK releases](https://github.com/mavlink/MAVSDK/releases) and install it using `dpkg`:
  ```
  sudo dpkg -i mavsdk_0.24.0_ubuntu18.04_amd64.deb
  ```
- **Fedora:** Download the matching **.rpm** file from [MAVSDK releases](https://github.com/mavlink/MAVSDK/releases) and install it using `rpm`:
  ```
  sudo rpm -U mavsdk-0.24.0-1.fc30-x86_64.rpm
  ```
- **Arch Linux:** Use `yay` to install the library from [AUR](https://aur.archlinux.org/packages/mavsdk/):
  ```yay
  yay -S mavsdk
  ```
- **Windows/Other platforms:** No binaries are available (yet) for other platforms, including Windows.
  For now you must [build the library from source](../contributing/build.md).


You will also to install a build toolchain to compile your code.

> **Note** You can use whatever build toolchain you like.
  Note however that this project uses `cmake` and the same compiler used to compile the flight stack software (generally you will install these when setting up the simulator).

  
## Setting up a Simulator

The easiest way to try out MAVSDK is to run one of the MAVSDK examples against a simulated version of the vehicle.

To set up and run the PX4 JMAVSim multicopter simulator on MacOS or Ubuntu:

1. First get the source code (you may need to install *git* for your platform):
   ```sh
   # Download PX4 Source Code
   git clone https://github.com/PX4/Firmware.git --recursive
   cd Firmware
   ```
1. Install the development environment for building the simulator:
   - **MacOS:**
     ```
     ulimit -S -n 2048

     # Common dependencies
     brew tap PX4/px4
     brew install px4-dev

     # Make sure you have Python 3 installed.
     brew install python3

     # install required packages using pip3
     pip3 install --user pyserial empy toml numpy pandas jinja2 pyyaml pyros-genmsg

     # Install SITL simulation with jMAVSim:
     brew tap AdoptOpenJDK/openjdk
     brew cask install adoptopenjdk8
     brew install px4-sim-jmavsim
     ```
   - **Ubuntu Linux (18.04):**
     ```sh
     bash ./Tools/setup/ubuntu.sh --no-nuttx
     ```
1. Run the simulator from the **Firmware** directory using the terminal:
   ```
   HEADLESS=1 make px4_sitl jmavsim 
   ```

> **Tip** For setting up the PX4 development environment/simulators on other platforms see [Setting up a Developer Environment (Toolchain)](https://dev.px4.io/master/en/setup/dev_env.html).
  PX4 supports a [number of simulators](https://dev.px4.io/en/simulation/) including [jMAVSim](https://dev.px4.io/en/simulation/jmavsim.html) and [Gazebo](https://dev.px4.io/en/simulation/gazebo.html).
  Use *Gazebo* if you need to simulate a VTOL or Fixed wing vehicle (jMAVSim can only simulate multicopters).


## Install QGroundControl

You can use *QGroundControl* to connect the simulator and observe vehicle movement and behaviour while the examples are running.
*QGroundControl* will automatically connect to the PX4 simulation as soon as it is started.

See [QGroundControl > Download and Install](https://docs.qgroundcontrol.com/en/getting_started/download_and_install.html) for information about setting up *QGroundControl* on your platform.


## Building the Takeoff Example {#build_examples}

The examples are stored as part of the projects source code.
Get them using *git* in a terminal:
```
git clone https://github.com/mavlink/MAVSDK.git --recursive
cd MAVSDK
```


Then build the example on macOS or Linux:
```sh
cd example/takeoff_land/
mkdir build && cd build
cmake ..
make
```


## Running the Example {#running_the_examples}

First start PX4 in SITL (Simulation) and *QGroundControl* as described above.

On Linux/macOS you can then run the example app (from the **example/takeoff_land/build** directory) as shown:
```sh
./takeoff_and_land udp://:14540
```

The MAVSDK application should connect to PX4, and you will be able to observe behaviour through the SDK terminal, SITL terminal, and/or *QGroundControl*.

> **Note** The first argument above is the connection string (`udp://:14540`).
  This is the standard PX4 UDP port for connecting to offboard APIs.

## Next Steps

Once MAVSDK is installed we recommend you:
- Try the [other examples](../examples/README.md)
- See the [Guide](../guide/README.md) for information about how to write MAVSDK apps using C++.