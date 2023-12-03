# src/Dockerfile
# Must start with a FROM instruction. It sets the Base Image (think OS) for the container:
FROM python:3.9-slim

# Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile
# For Streamlit 1.10.0 or higher, you must set the WORKDIR to a directory other than the root directory
WORKDIR /src

# Install essential utilities including git
# After installing these packages, cleans up the package lists to minimize the size
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    software-properties-common \
    git \
    && rm -rf /var/lib/apt/lists/*

# Clone your code that lives in a remote public repo (url) to WORKDIR (.)
# RUN git clone https://github.com/streamlit/streamlit-example.git .

# Local Copy
# If your code lives in the same directory as the Dockerfile, copy all your src files from your server into the container WORKINGDIR
# Syntax: COPY <src-path> <destination-path>
COPY . .

# Create a virtual environment
RUN python -m venv /venv
# Activate the virtual environment
ENV PATH="/venv/bin:$PATH"

# Install your app’s Python dependencies from the cloned requirements.txt in the container
RUN pip install --upgrade pip
RUN pip install -r requirements.txt

# Container will listen to Streamlit’s (default) port 8501
EXPOSE 8501
# tells Docker how to test a container to check that it is still working
HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

ENTRYPOINT ["streamlit", "run", "webapp/app.py", "--server.port=8501", "--server.address=0.0.0.0"]