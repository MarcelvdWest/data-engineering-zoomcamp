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
WORKDIR /code

# Add virtual environment to PATH so we can use installed packages
ENV PATH="/code/.venv/bin:$PATH"

# Copy dependency files first (better layer caching)
COPY pyproject.toml .python-version uv.lock ./
# Install only runtime dependencies from the lock file for a smaller image
RUN uv sync --locked

# Copy application code
COPY ingest_data.py .

# Set entry point
ENTRYPOINT ["uv", "run", "python", "ingest_data.py"]
