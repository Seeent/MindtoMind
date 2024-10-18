# FROM mcr.microsoft.com/devcontainers/python:0-3.10

# ENV PYTHONUNBUFFERED 1

# # [Optional] If your requirements rarely change, uncomment this section to add them to the image.
# # COPY requirements.txt /tmp/pip-tmp/
# # RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt \
# #    && rm -rf /tmp/pip-tmp

# #[Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends postgresql-client



# Build stage (para frontend)
FROM node:16 AS frontend-builder

WORKDIR /app

# Copiamos los archivos del frontend
COPY ./frontend ./frontend

# Instalamos dependencias y construimos el frontend
RUN cd frontend && npm install && npm run build

# Backend stage (para Python)
FROM mcr.microsoft.com/devcontainers/python:0-3.10

ENV PYTHONUNBUFFERED 1

# Copiamos los archivos del backend
COPY ./backend ./backend

# Instalamos las dependencias del backend
WORKDIR /backend
RUN pip install -r requirements.txt

# Copiamos el build del frontend al directorio estático del backend (si corresponde)
COPY --from=frontend-builder /app/frontend/build ./backend/static

# Si es necesario, instala PostgreSQL cliente
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends postgresql-client

CMD ["python", "src/app.py"]
