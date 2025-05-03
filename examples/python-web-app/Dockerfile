# ---- Stage 1: Builder ----
FROM python:3.10-slim AS builder

WORKDIR /app

# Install build dependencies
COPY requirements.txt .
RUN python3 -m venv /venv && \
    /venv/bin/pip install --no-cache-dir -r requirements.txt

COPY devops/ /app/

# ---- Stage 2: Distroless ----
FROM gcr.io/distroless/python3-debian11

# Copy app and venv from builder
COPY --from=builder /app /app
COPY --from=builder /venv /venv

WORKDIR /app

# Set Python path to use the virtual environment
ENV PATH="/venv/bin:$PATH"

EXPOSE 8000

CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
