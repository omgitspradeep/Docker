# --- Build Stage ---
FROM ubuntu AS builder

WORKDIR /app

COPY requirements.txt .
COPY devops ./devops

RUN python3 -m venv /venv && /venv/bin/pip install --no-cache-dir -r requirements.txt

# --- Production Stage (Distroless) ---
FROM gcr.io/distroless/python3-debian11

WORKDIR /app

COPY --from=builder /venv /venv
COPY devops ./devops
COPY requirements.txt .

ENV PATH="/venv/bin:$PATH"

EXPOSE 8000

CMD ["python3", "devops/manage.py", "runserver", "0.0.0.0:8000"]
