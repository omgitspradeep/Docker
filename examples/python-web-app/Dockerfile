# ====================
# Stage 1: Build stage
# ====================
FROM python:3.11-slim AS builder

# Set working directory
WORKDIR /app

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Copy only requirements first for better caching
COPY requirements.txt .

# Create and activate virtual environment, install dependencies
RUN python -m venv /opt/venv && \
    /opt/venv/bin/pip install --upgrade pip && \
    /opt/venv/bin/pip install --no-cache-dir -r requirements.txt

# Copy the entire project (assuming manage.py is inside devops/)
COPY devops/ /app/devops/

# =====================
# Stage 2: Runtime stage
# =====================
FROM gcr.io/distroless/python3

# Set the working directory to the Django project folder
WORKDIR /app/devops

# Copy installed virtual environment from builder stage
COPY --from=builder /opt/venv /opt/venv

# Copy application code
COPY --from=builder /app/devops /app/devops

# Use virtual environment binaries by default
ENV PATH="/opt/venv/bin:$PATH"

# Expose Django dev server port
EXPOSE 8000

# Use JSON-form CMD to avoid distroless shell issues
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
