# Installation Guide

This guide provides step-by-step instructions for setting up the Graph Coloring with RL project.

## Prerequisites

### System Requirements

- **Operating System:** Linux (tested on Ubuntu 18.04+), macOS, or Windows
- **Python:** 3.9 or higher (3.9, 3.10, 3.11, or 3.12)
- **G++:** Compiler with C++17 support
- **OpenMP:** For parallel processing in C++ code

### Installing System Dependencies

#### Ubuntu/Debian

```bash
sudo apt-get update
sudo apt-get install -y python3 python3-pip python3-venv g++ libomp-dev make
```

#### macOS

```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install dependencies
brew install python gcc libomp
```

#### Windows

1. Install [Python 3.9+](https://www.python.org/downloads/)
2. Install [MinGW-w64](https://www.mingw-w64.org/) or [Visual Studio Build Tools](https://visualstudio.microsoft.com/downloads/)
3. OpenMP is included with MSVC or MinGW-w64

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/NP-compute/Graph_Coloring_with_RL.git
cd Graph_Coloring_with_RL
```

### 2. Create a Virtual Environment (Recommended)

Using a virtual environment isolates project dependencies from your system Python.

```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
# On Linux/macOS:
source venv/bin/activate

# On Windows:
venv\Scripts\activate
```

### 3. Install Python Dependencies

#### Option A: Using pip and requirements.txt

```bash
pip install -r requirements.txt
```

#### Option B: Using pip and pyproject.toml

```bash
pip install -e .
```

#### Option C: Manual Installation

```bash
pip install "torch>=2.0.0,<3.0.0" "numpy>=1.23.0,<2.1.0"
```

### 4. Verify Installation

Check that PyTorch and NumPy are installed correctly:

```bash
python3 -c "import torch; import numpy; print(f'PyTorch: {torch.__version__}, NumPy: {numpy.__version__}')"
```

Expected output (versions may vary):
```
PyTorch: 2.x.x, NumPy: 1.xx.x
```

### 5. Build C++ Shared Library

The project uses C++ code for graph operations, which needs to be compiled:

```bash
cd DQNet
make
```

If successful, you should see the training process start.

## Troubleshooting

### PyTorch Installation Issues

If you encounter issues installing PyTorch, visit the [official PyTorch installation guide](https://pytorch.org/get-started/locally/) for platform-specific instructions.

For CPU-only installation:
```bash
pip install torch --index-url https://download.pytorch.org/whl/cpu
```

For CUDA-enabled GPU:
```bash
# Check CUDA version first
nvidia-smi

# Install PyTorch with matching CUDA version (example for CUDA 11.8)
pip install torch --index-url https://download.pytorch.org/whl/cu118
```

### OpenMP Not Found

**Linux:**
```bash
sudo apt-get install libomp-dev
```

**macOS:**
```bash
brew install libomp
```

If you still get errors, you may need to set environment variables:
```bash
export OMP_NUM_THREADS=4
```

**Windows:**
Ensure you're using MinGW-w64 or MSVC which includes OpenMP support.

### G++ Compilation Errors

Ensure you have a C++17-compatible compiler:

```bash
g++ --version
# Should show version 7.0 or higher
```

### Python Version Issues

Check your Python version:
```bash
python3 --version
```

If you have an older version, install Python 3.9+ from [python.org](https://www.python.org/downloads/).

### Shared Library Path Issues

If you see errors about `lib_graph.so` not being found:

**Linux/macOS:**
```bash
cd DQNet
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:./source
python3 main.py
```

**Windows:** The Makefile may need modification to use `.dll` instead of `.so`.

## Optional: CPLEX Installation

For optimal coloring algorithm (optional):

1. Download CPLEX from [IBM ILOG CPLEX Optimization Studio](https://www.ibm.com/products/ilog-cplex-optimization-studio)
2. Follow IBM's installation instructions
3. Academic licenses are available for free

## Verifying the Setup

To verify everything is working correctly:

```bash
cd DQNet

# Test compilation
make clean  # If exists
g++ --version
python3 -c "import torch; import numpy; print('Dependencies OK')"

# Run a quick test (this will start training)
make
```

## Development Setup

If you plan to contribute to the project:

```bash
# Install development dependencies
pip install -e ".[dev]"

# Run code formatter
black DQNet/

# Run linter
flake8 DQNet/

# Run tests (if available)
pytest
```

## Updating Dependencies

To update to the latest compatible versions:

```bash
pip install --upgrade torch numpy
```

To check for outdated packages:

```bash
pip list --outdated
```

## Uninstallation

To remove the virtual environment and all dependencies:

```bash
# Deactivate virtual environment
deactivate

# Remove virtual environment directory
rm -rf venv

# Remove compiled C++ files
cd DQNet
rm -f source/*.o source/*.so
```

## Getting Help

If you encounter issues:

1. Check the [DEPENDENCY_AUDIT.md](DEPENDENCY_AUDIT.md) for known issues
2. Open an issue on [GitHub](https://github.com/NP-compute/Graph_Coloring_with_RL/issues)
3. Ensure you're using compatible Python (3.9-3.12) and PyTorch (2.x) versions

## Next Steps

After successful installation:

1. Read the [README.md](README.md) for project overview
2. Check the project report at `Docs/Project Report.pdf`
3. Explore the `DQNet/` directory for the main implementation
4. Review the `CSV/` directory for experiment results
