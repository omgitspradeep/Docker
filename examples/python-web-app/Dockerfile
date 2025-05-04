# --- Stage 1: Build dependencies and virtual environment ---
FROM python:3.12-slim AS builder

WORKDIR /app

# Install build dependencies (if needed, e.g., for compiling packages)
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt /app/
COPY devops /app/

# Create virtual environment and install dependencies
RUN python -m venv /app/venv && \
    /app/venv/bin/pip install --upgrade pip && \
    /app/venv/bin/pip install --no-cache-dir -r requirements.txt

# --- Stage 2: Final, minimal image ---
FROM python:3.12-slim

WORKDIR /app

# Copy only the virtual environment and app code from builder stage
COPY --from=builder /app/venv /app/venv
COPY --from=builder /app/devops /app/devops
COPY --from=builder /app/requirements.txt /app/requirements.txt

EXPOSE 8000

# Use the venv's Python directly (no need to activate)
CMD ["/app/venv/bin/python", "devops/manage.py", "runserver", "0.0.0.0:8000"]
