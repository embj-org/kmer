# K-mer

A collection of k-mer counting implementations designed for performance testing and benchmarking across different embedded hardware platforms. This project serves as a demonstration application for the [EJ (Embedded Job) testing framework](https://github.com/embj-org/ej).

## What is K-mer?

K-mer algorithms are commonly used in bioinformatics for analyzing DNA sequences by counting occurrences of subsequences of length k. For this benchmark, we use the digits of PI as our input sequence since it provides a deterministic, easily reproducible dataset that still demonstrates the algorithm's computational characteristics.

Learn more about k-mers on [Wikipedia](https://en.wikipedia.org/wiki/K-mer).

## Implementations

This repository contains three versions of the k-mer counting algorithm:

- **k-mer-original**: Unoptimized baseline implementation
- **k-mer**: Single-threaded optimized version
- **k-mer-omp**: Multi-threaded optimized version using OpenMP

Each implementation processes the same input and produces identical results, allowing for direct performance comparisons across different optimization levels and hardware platforms.

## Building

### Prerequisites

- CMake 3.10 or higher
- C compiler with C99 support
- OpenMP support (for the multi-threaded version)

### Cross-compilation for ARM64

For cross-compilation to ARM64 (e.g., Raspberry Pi), you'll need the AArch64 GNU/Linux toolchain:

```bash
# Download and extract the toolchain
wget https://developer.arm.com/-/media/Files/downloads/gnu-a/10.3-2021.07/binrel/gcc-arm-10.3-2021.07-x86_64-aarch64-none-linux-gnu.tar.xz
tar -xf gcc-arm-10.3-2021.07-x86_64-aarch64-none-linux-gnu.tar.xz
export PATH=$PWD/gcc-arm-10.3-2021.07-x86_64-aarch64-none-linux-gnu/bin:$PATH
```

### Build Commands

```bash
# Native build
cmake -B build
cmake --build build -j$(nproc)

# Cross-compile for ARM64
cmake -B build-pi -DCMAKE_TOOLCHAIN_FILE=aarch64_toolchain.cmake
cmake --build build-pi -j$(nproc)
```

## Usage

All implementations follow the same command-line interface:

```bash
./k-mer-original <input_file> <k_value>
./k-mer <input_file> <k_value>
./k-mer-omp <input_file> <k_value>
```

### Example

```bash
# Count 3-mers in the provided PI digits file
./k-mer-original inputs/inputs.txt 3
```

Output format:

```
Results:
ABC: 2
BCD: 1
CDA: 1
DAB: 1
```

## Input Files

The `inputs/` directory contains sample data files:

- `input.txt`: Alternative input file for testing
- `pi_dec_1k.txt`: First 1000 decimal digits of PI
- `pi_dec_1m.txt`: First 1'000'000 decimal digits of PI
- `pi_hex_1m.txt`: First 1'000'000 hexadecimal digits of PI

## Integration with EJ

For a complete guide on using this project with EJ, see the [EJ Documentation](https://embj-org.github.io/ej/).

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Related Projects

- [EJ (Embedded Job)](https://github.com/embj-org/ej) - The testing framework this project demonstrates
- [EJ Builder SDK](https://crates.io/crates/ej-builder-sdk) - SDK for building EJ-compatible tools
- [EJ Kmer Builder](https://github.com/embj-org/ejkmer-builder) - Example EJ Builder implementation
- [EJ Kmer Dispatcher](https://github.com/embj-org/ejkmer-dispatcher) - Example EJ Dispatcher SDK usage
