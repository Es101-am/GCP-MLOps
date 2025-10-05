
# Use Google's base image for CPU ML workloads
FROM gcr.io/deeplearning-platform-release/base-cpu

# Define working directory
WORKDIR /mlops-cli

# Copy your pipeline script and dependency list
COPY pipeline.py ./pipeline.py
COPY requirements.txt ./requirements.txt

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Default command: execute the pipeline script
ENTRYPOINT ["python", "pipeline.py"]
