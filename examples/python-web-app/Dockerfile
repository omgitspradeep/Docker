# ====================
# Stage 1: Build stage
# ====================
FROM python:3.11-slim AS builder

WORKDIR /app

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends build-essential && rm -rf /var/lib/apt/lists/*

COPY requirements.txt ./

# Create virtual environment and install dependencies
RUN python -m venv /opt/venv && /opt/venv/bin/pip install --upgrade pip && /opt/venv/bin/pip install --no-cache-dir -r requirements.txt

# =====================
# Stage 2: Runtime stage
# =====================
#FROM python:3.11-slim
FROM gcr.io/distroless/python3

WORKDIR /app

# Copy only necessary files from builder
COPY --from=builder /opt/venv /opt/venv
COPY devops /app/
COPY requirements.txt /app/

# Use the virtual environment by default
ENV PATH="/opt/venv/bin:$PATH"

EXPOSE 8000

CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
