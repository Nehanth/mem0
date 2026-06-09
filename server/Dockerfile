FROM python:3.12-slim-bookworm

WORKDIR /app

# Install libpq runtime for psycopg python implementation fallback
RUN apt-get update && \
    apt-get install -y --no-install-recommends libpq5 && \
    rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt && \
    pip install --no-cache-dir "psycopg[binary]"

COPY . .

# Create writable directories for OpenShift (random UID, GID 0)
ENV HOME=/opt/app-root
ENV MEM0_DIR=/opt/app-root/.mem0
ENV HISTORY_DB_PATH=/opt/app-root/history/history.db
ENV PYTHONUNBUFFERED=1

RUN mkdir -p /opt/app-root/.mem0 /opt/app-root/history /opt/app-root/.cache && \
    chgrp -R 0 /opt/app-root /app && \
    chmod -R g=u /opt/app-root /app

EXPOSE 8000

USER 1001

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
