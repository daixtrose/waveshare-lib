# libwaveshare

C++23 library for Waveshare serial server devices.

Provides:
- **VirCom UDP network scanning** — discover devices on the local network
- **Device configuration** — set static IP, DHCP, Modbus TCP, port, device name
- **Modbus TCP connection factory** — create preconfigured `libmodbus_cpp::ModbusConnection` instances

## Dependencies

- [libmodbus-cpp](https://github.com/daixtrose/libmodbus-cpp) (fetched automatically via CMake FetchContent)

## Building

```bash
cmake --preset linux-x86_64-debug
cmake --build build/linux-x86_64-debug -j$(nproc)
```

## Usage as a CMake dependency

```cmake
# Via FetchContent
FetchContent_Declare(
    libwaveshare_proj
    GIT_REPOSITORY https://github.com/daixtrose/libwaveshare.git
    GIT_TAG main
    GIT_SHALLOW TRUE
)
FetchContent_MakeAvailable(libwaveshare_proj)

target_link_libraries(your_target PRIVATE waveshare)
```

## Logging

All functions that produce debug output accept an optional `waveshare::LogFn`
callback parameter.  When provided, output is routed through the callback
instead of being printed to stdout/stderr.  This allows callers (e.g. a gRPC
service) to redirect debug output to their own logging infrastructure.

```cpp
#include <libwaveshare/network_scanner.hpp>

// Route debug output to your own logger
auto devices = waveshare::scan_network(3000, /*debug=*/true, {}, {},
    [](std::string msg) { my_logger.debug(msg); });
```

## License

MIT
