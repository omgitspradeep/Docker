# --- Stage 1: Build Stage ---
FROM ubuntu:latest AS builder

WORKDIR /app

# Install Python and pip
RUN apt-get update && \
    apt-get install -y python3 python3-pip python3-venv

# Copy requirements and source code
COPY requirements.txt /app/
COPY devops /app/

# Create virtual environment and install dependencies
RUN python3 -m venv /app/venv && \
    /app/venv/bin/pip install --no-cache-dir -r requirements.txt

# --- Stage 2: Final Image ---
FROM ubuntu:latest

WORKDIR /app

# Install only Python (no pip or venv needed in production)
RUN apt-get update && \
    apt-get install -y python3 && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Copy virtual environment and app from builder
COPY --from=builder /app/venv /app/venv
COPY --from=builder /app/devops /app/devops
COPY --from=builder /app/requirements.txt /app/requirements.txt

EXPOSE 8000

# Use venv's python interpreter directly (no need to activate)
CMD ["/app/venv/bin/python", "devops/manage.py", "runserver", "0.0.0.0:8000"]
