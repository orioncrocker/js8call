
# JS8Call Installation Guide (Linux/macOS, Boost 1.88, Qt6)

JS8Call is built atop the Qt6 framework and can be compiled on Linux and macOS.
This guide assumes you’re using a modern Debian‑based distro (Ubuntu 22.04+), or macOS with Homebrew.

---
## 1. Install System Dependencies

### On **Ubuntu / Debian**
```bash
sudo apt update
sudo apt install -y git cmake build-essential pkg-config qt6-base-dev qt6-multimedia-dev qt6-tools-dev qt6-l10n-tools qt6-serialport-dev libhamlib-dev libfftw3-dev libusb-1.0-0-dev
``````

### On **macOS**
```bash
brew update
brew install git cmake pkg-config
brew install qt6 hamlib fftw
brew link --force qt6
```

## 2. Download & Build Boost
Boost 1.77 is required for the build, and system packages may be too old. The following installs the latest version (1.88 at the time of writing this), but any version at or after 1.77 will do.

```bash
wget https://archives.boost.io/release/1.88.0/source/boost_1_88_0.tar.gz
tar -xzf boost_1_88_0.tar.gz
cd boost_1_88_0
./bootstrap.sh --prefix="$BOOST_DIRECTORY" # you'll have to figure out where you want this
./b2 install
```

## 3. Compile JS8Call

```bash
git clone https://github.com/js8call/js8call.git
cd js8call
# (Optional) Checkout a specific release
```

## 4. Build JS8Call

### On Linux
```bash
mkdir -p build && cd build
cmake .. \
  -DCMAKE_PREFIX_PATH=/usr/lib/qt6 \
  -DBOOST_ROOT="$BOOST_DIRECTORY" \
  -DBoost_NO_SYSTEM_PATHS=ON
make -j"$(nproc)"
```

### On macOS
```bash
mkdir -p build && cd build
cmake .. \
  -DCMAKE_PREFIX_PATH="$(brew --prefix qt6)" \
  -DBOOST_ROOT="$BOOST_DIRECTORY" \
  -DBoost_NO_SYSTEM_PATHS=ON
make -j"$(sysctl -n hw.ncpu)"
```

From here you have multiple options. You can either build the software as a package, install the executable, or simply run it from the current directory

### 4.1 Package (.deb, .rpm, etc)

```bash
make package
sudo apt install ./js8call*.deb
```

### 4.2 Install executable

```bash
make install # note, this may need sudo!
```

## 5. Run JS8Call

This can be either run from the local direc using `./js8call` or `js8call` depending on what you did above
