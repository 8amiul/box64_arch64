To cross-compile **Box64** for an ARM64 (AArch64) device on an Arch Linux system, you'll need to set up a cross-compilation toolchain, fetch the required dependencies, and configure the build process correctly for ARM64. Here's a step-by-step guide:

### 1. Install Required Dependencies
Ensure you have the necessary build tools and dependencies installed on your Arch Linux system.

```bash
sudo pacman -S git cmake gcc gcc-aarch64-linux-gnu
```

This installs `git`, `cmake`, and the `gcc` cross-compiler for the ARM64 architecture.

### 2. Clone the Box64 Repository
Get the latest version of **Box64** from its GitHub repository.

```bash
git clone https://github.com/ptitSeb/box64.git
cd box64
```

### 3. Configure CMake for Cross Compilation
Set up the build directory and tell CMake that you are targeting ARM64:

```bash
mkdir build
cd build
```

Configure the build with `CMake` and specify the cross-compiler:

```bash
cmake .. -DCMAKE_BUILD_TYPE=Release -DARM_DYNAREC=ON \
    -DCMAKE_TOOLCHAIN_FILE=../cmake/aarch64-toolchain.cmake
```

If there isn't a toolchain file in the Box64 repository, you can create your own `aarch64-toolchain.cmake` file.

### Example of Toolchain File (`aarch64-toolchain.cmake`)
```cmake
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR aarch64)

# Cross compiler settings
set(CMAKE_C_COMPILER aarch64-linux-gnu-gcc)
set(CMAKE_CXX_COMPILER aarch64-linux-gnu-g++)

# Target architecture
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
```

### 4. Compile Box64
Now, you can compile the source code for the ARM64 architecture:

```bash
make -j$(nproc)
```

This command will build Box64 using all available CPU cores.

### 5. Transfer to ARM64 Device
Once the build is complete, you can copy the binary to your ARM64 device using `scp` or another method.

```bash
scp box64 aarch64-user@arm-device-ip:/path/to/destination
```

### 6. Run Box64 on the ARM64 Device
Log in to your ARM64 device and ensure that any necessary dependencies for running Box64 are installed. You can run the binary as follows:

```bash
./box64
```

### Notes:
- **Dynarec on ARM64:** If you want to enable dynamic recompilation for better performance, make sure `-DARM_DYNAREC=ON` is included in the `cmake` configuration step.
- You may need additional libraries (e.g., SDL2) depending on your use case, so ensure you install the required dependencies on both your Arch Linux build system and the ARM64 device.

Let me know if you run into any issues during the process!
