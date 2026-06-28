## Patch for HIPv2

# Installation

## macOS

For MacOS perform the following:

First, install the needed libraries
`brew install c-ares cmake glib gnutls lua qt6`

Then, export the path to QT6

`export PATH=/opt/homebrew/opt/qt6/bin:$PATH`

Clone the repo:

`git clone https://gitlab.com/wireshark/wireshark.git`

Test the patch:

`cd wireshark && patch -p1 --dry-run < packet-hip.patch`

Apply the patch:

`patch -p1 < packet-hip.patch`

Finally, build the binaries:

`mkdir build && cd build && cmake ../ && make`

## Ubuntu

```bash
# 1. clone
git clone https://gitlab.com/wireshark/wireshark.git
cd wireshark

# 2. install build dependencies (Wireshark's own helper)
sudo tools/debian-setup.sh --install-optional
#    pulls in: cmake, build-essential, qt6 dev packages, glib, c-ares,
#    gnutls, lua, libpcap, flex, bison, etc.

# 3. apply the patch (same commands as macOS)
patch -p1 --dry-run < packet-hip.patch    # verify it applies cleanly
patch -p1 < packet-hip.patch
#    then add the 0x0c GCM line(s) above to epan/dissectors/packet-hip.c

# 4. build
mkdir build && cd build
cmake ..
make -j$(nproc)            # or: cmake .. -G Ninja && ninja

# 5. run
./run/wireshark
```

# Re-applying the patch

You can't re-apply on top of an already-patched file. The fix is to restore the
file to pristine and apply the updated patch once.

Since you cloned wireshark with git, restore the dissector and re-apply:

```bash
cd /path/to/wireshark

# 1. restore packet-hip.c to its clean upstream state (undo the old apply)
git checkout -- epan/dissectors/packet-hip.c
git status        # should show a clean working tree for that file

# 2. apply the UPDATED patch.
#    Point at its real path so you don't accidentally use an old copy:
patch -p1 --dry-run < /path/to/packet-hip.patch
patch -p1         < /path/to/packet-hip.patch

# 3. rebuild
cd build && make -j$(nproc)
```

# Running the binary

And now you can run the Wireshark:

`./run/wireshark`
