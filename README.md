## Patch for HIPv2

# Installation
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

# Running the binary

And now you can run the Wireshark:

`./run/wireshark`

