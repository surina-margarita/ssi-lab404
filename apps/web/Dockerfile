FROM dhi.io/python:3.14-dev@sha256:0fc1a06a500fd1a1a884d7bb53c8da242527878e39eefcf106ed8050e94693ad AS builder
WORKDIR /app

RUN python -m venv /venv
ENV PATH="/venv/bin:$PATH"

COPY requirements.txt .
RUN pip install --no-cache-dir --no-compile -r requirements.txt && python -m pip uninstall --yes pip

COPY . .

FROM dhi.io/python:3.14@sha256:36c38f4be040b095f67fbb0982d18875518f1b58d23b9ad3cc8bc903a7f7524e

WORKDIR /app

ENV PATH="/venv/bin:$PATH" \
    OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1
COPY --from=builder --chown=1000:1000 /venv /venv
COPY --chown=1000:1000 app.py .
COPY --chown=1000:1000 templates ./templates

USER 1000:1000

EXPOSE 8080

CMD ["opentelemetry-instrument", "gunicorn", "--bind=0.0.0.0:8080", "--access-logfile=-", "app:app"]