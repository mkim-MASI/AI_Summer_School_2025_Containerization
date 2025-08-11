# Base image: lightweight Python 3.11
FROM python:3.11-slim

# Avoid interactive prompts during install
ENV DEBIAN_FRONTEND=noninteractive

# Install basic system dependencies
RUN apt-get update && apt-get install -y \
    git \
    wget \
    curl \
    build-essential vim \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN pip install --upgrade pip

# Install requested Python packages with specified versions
RUN pip install \
    "datasets>=4.0.0" \
    "kagglehub>=0.3.12" \
    "matplotlib>=3.10.5" \
    "torch>=2.8.0" \
    "torchvision>=0.23.0"

# Set working directory
WORKDIR /workspace

# Import the python test file
COPY run_python_test.py /workspace/

# Default command
CMD ["python3", "/workspace/run_python_test.py"]
