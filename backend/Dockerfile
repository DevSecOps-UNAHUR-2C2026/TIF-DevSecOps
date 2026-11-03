# ── Stage 1: builder ────────────────────────────────────
FROM python:3.12-slim AS builder

WORKDIR /build
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# ── Stage 2: imagen final (sin herramientas de build) ───
FROM python:3.12-slim AS final

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PORT=5000

WORKDIR /app

# Copiar solo las dependencias instaladas desde el builder
COPY --from=builder /install /usr/local

# Copiar el código
COPY app.py .

# Script de inicio que espera a que Postgres esté listo
COPY entrypoint.sh .
RUN chmod +x entrypoint.sh

# Usuario no-root con UID numérico (OWASP ASVS V5.2.1)
RUN groupadd -g 10001 appuser && \
    useradd -u 10001 -g appuser --create-home -d /app appuser && \
    chown -R appuser:appuser /app

USER 10001

EXPOSE 5000
ENTRYPOINT ["./entrypoint.sh"]
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "2", "app:app"]
