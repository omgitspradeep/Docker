# --- Build Stage ---
FROM ubuntu AS builder

WORKDIR /app

COPY requirements.txt .
COPY devops ./devops

RUN apt-get update && apt-get install -y python3 python3-pip python3-venv

# --- Production Stage (Distroless) ---
FROM gcr.io/distroless/python3-debian11

WORKDIR /app

COPY --from=builder /venv /venv
COPY devops ./devops
COPY requirements.txt .

ENV PATH="/venv/bin:$PATH"

EXPOSE 8000

CMD ["python3", "devops/manage.py", "runserver", "0.0.0.0:8000"]
