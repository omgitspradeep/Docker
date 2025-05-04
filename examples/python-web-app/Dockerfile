# ====================
# Stage 1: Build stage
# ====================
FROM python:3.11-slim AS builder

WORKDIR /app

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install dependencies into a virtual environment
COPY requirements.txt .
RUN python -m venv /opt/venv && \
    /opt/venv/bin/pip install --upgrade pip && \
    /opt/venv/bin/pip install --no-cache-dir -r requirements.txt

# =====================
# Stage 2: Runtime stage
# =====================
FROM gcr.io/distroless/python3

# Set working directory to the Django project inside devops/
WORKDIR /app/devops

# Copy the virtual environment
COPY --from=builder /opt/venv /opt/venv

# Copy Django project files from devops/
COPY devops/ /app/devops/

# Set the virtual environment's bin folder in the PATH
ENV PATH="/opt/venv/bin:$PATH"

EXPOSE 8000

# Run Django development server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
