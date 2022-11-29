## 5. Tools

1. *Development environments*
    - Adoption of a specific development environment will not be mandated at the company level.  Individuals are free to use their tool of choice unless specific Teams or Projects mandate a particular tool at the Team or Project level.
    - [AMD/Xilinx Vivado](https://www.xilinx.com/products/design-tools/vivado.html) is preferred for FPGA block designs and coding in Verilog and VHDL.
    - In the absence of any other personal/Team/Project preference, [VSCode](https://code.visualstudio.com/) is recommended as it is used fairly widely across the different Teams.
    - All repo readmes should specify how to develop the project using at least one IDE, including instructions for accessing documentation, building and running from within the chose IDE.

2. *Default technologies*
This section contains a collection of *recommended* frameworks/tools/libraries (so-called *technologies*) for certain applications. As such, it is not a requirement to adopt these specific technologies, i.e., subject-matter experts in each team may have their own preference.

    a. REST server: [fastAPI](https://fastapi.tiangolo.com/) (Python)

      - FastAPI is the preferred technology for implementing REST servers in Python.
      - No default technology recommendations for other languages.

    b. WebApp: [Svelte](https://github.com/sveltejs/svelte) & [MaterialUI](https://mui.com/)
    The reason for this recommendation is that there are examples of WebApps in QB using Svelte.

    c. GUI: [wxPython](https://www.wxpython.org/)
    The reason for this recommendation is that there are examples in QB of GUIs (diamond characterisation) implemented using wxPython. The wxPython package has an open source licence similar to LGPL, allowing linking proprietary code and distrubuting binaries.

    d. Test frameworks:

      - C++ tests: GoogleTest (`gtest`)
          - includes `gmock`
      - Linux profiling: `gprof`
      - Python tests: `pytest`
      - CI tests should be run using Gitlab CI.

    e. Embedded operating system:

    Linux is preferred for embedded (ARM) applications such as on Xilinx hardware and Raspberry Pis.  64-bit aarch64 Debian derivates preferred (Raspios, Ubuntu), or vendor distributions such as [AMD/Xilinx Petalinux](https://www.xilinx.com/products/design-tools/embedded-software/petalinux-sdk.html). 

    f. Containerisation

    [Docker](https://www.docker.com/) and docker-compose.
    However note that the docker-desktop UI is not licensed for commercial use. 

    g. Network protocol and RPC

    [Cap'n Proto](https://capnproto.org/) is preferred when a REST API is not appropriate.

    h. C and C++ build systems

    [CMake](https://www.cmake.org) is preferred.


