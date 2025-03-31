# Conda Cheat Sheet

## Installation
```bash
# Download and install Miniconda (minimal installer)
# https://docs.conda.io/en/latest/miniconda.html

# Verify installation
conda --version
```

## Environment Management
```bash
# Create a new environment
conda create --name myenv

# Create environment with specific Python version
conda create --name myenv python=3.9

# Create environment from environment.yml file
conda env create -f environment.yml

# Activate environment
conda activate myenv

# Deactivate current environment
conda deactivate

# List all environments
conda env list

# Remove an environment
conda env remove --name myenv
```

## Package Management
```bash
# Install a package in active environment
conda install package_name

# Install specific version
conda install package_name=1.2.3

# Install multiple packages
conda install package1 package2

# Install from specific channel
conda install -c conda-forge package_name

# Update a package
conda update package_name

# Update all packages in environment
conda update --all

# List installed packages
conda list

# Search for a package
conda search package_name

# Remove a package
conda remove package_name
```

## Environment Export/Import
```bash
# Export environment to YAML file
conda env export > environment.yml

# Export environment without build specifications
conda env export --from-history > environment.yml

# Clone an environment
conda create --name newenv --clone oldenv
```

## Channels and Config
```bash
# Add a channel
conda config --add channels conda-forge

# Set channel priority
conda config --set channel_priority strict

# Show config
conda config --show

# Reset config
conda config --remove-key KEY_NAME
```

## Conda Updates
```bash
# Update conda itself
conda update conda

# Clean unused packages and caches
conda clean --all
```

## Common Options
```bash
# Install without asking for confirmation (-y)
conda install -y package_name

# Create environment with specific packages
conda create -n myenv python=3.9 numpy pandas

# Verbose output for troubleshooting
conda install -v package_name
```

## Working with pip
```bash
# Install pip in conda environment
conda install pip

# Use pip to install packages not available in conda
pip install package_name

# Generate requirements.txt from pip packages
pip freeze > requirements.txt
```

## Troubleshooting
```bash
# Fix broken environment
conda update --all

# Recreate environment from scratch
conda env remove --name myenv
conda env create -f environment.yml

# Enable/disable auto-activation of base environment
conda config --set auto_activate_base False
conda config --set auto_activate_base True
```

## Conda vs. conda-forge
- **conda**: Default channel maintained by Anaconda
- **conda-forge**: Community-led channel with more up-to-date packages
```bash
# Install preferring conda-forge packages
conda install -c conda-forge package_name
```
