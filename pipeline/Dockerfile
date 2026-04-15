# ## USING PIP
# # Docker image Base
# FROM python:3.13.11-slim

# # Install neccesary packages
# RUN pip install pandas pyarrow

# # Set working dir
# WORKDIR /app
# # copy script to the container. 
# COPY pipeline.py pipeline.py

# # define what to do first when the container runs
# # for example,  here we will just run the scripts
# ENTRYPOINT ["python", "pipeline.py"]

## USING UV (Virtual Environment)
# Docker image Base
FROM python:3.13.11-slim

# Copy uv binary from official uv image (multi-stage build pattern)
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/

# Set working directory
WORKDIR /app

# Add virtual environment to PATH so we can use installed packages
ENV PATH="/app/.venv/bin:$PATH"

# Copy dependency files first (better layer caching)
COPY "pyproject.toml" "uv.lock" ".python-version" ./
# Install dependencies from lock file (ensures reproducible builds)
RUN uv sync --locked

# Copy application code
COPY pipeline.py pipeline.py

# Set entry point
ENTRYPOINT ["uv", "run", "python", "pipeline.py"]
