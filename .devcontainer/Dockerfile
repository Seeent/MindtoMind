# Build stage (para frontend)
FROM node:16 AS frontend-builder

WORKDIR /app

# Copiamos los archivos del frontend
COPY ./frontend ./frontend

# Instalamos dependencias y construimos el frontend
RUN npm install && npm run build

# Verificar variables de entorno
RUN printenv

# Backend stage (para Python)
FROM mcr.microsoft.com/devcontainers/python:0-3.10

ENV PYTHONUNBUFFERED 1

ARG NODE_ENV
ENV NODE_ENV $NODE_ENV


# Copiamos los archivos del backend
COPY ./backend ./backend

# Copiamos archivo .env
COPY .env /app/.env

# Instalamos las dependencias del backend
WORKDIR /backend
RUN pip install -r requirements.txt

# Copiamos el build del frontend al directorio estático del backend (si corresponde)
COPY --from=frontend-builder /app/frontend/build ./backend/static

# Si es necesario, instala PostgreSQL cliente
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends postgresql-client

CMD ["python", "src/app.py"]
