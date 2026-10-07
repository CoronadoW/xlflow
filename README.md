# XLFlow (EsflowiX) - Deploy

Este repositorio contiene la configuración de Docker Compose para desplegar XLFlow.

## Estructura
EsflowiX/
├── docker-compose.yml ← Orquestación de contenedores
├── .env ← Variables de entorno (NO subir a Git)
├── .env.example ← Plantilla del .env
├── .gitignore ← Excluye back, front y .env
├── README.md ← Este archivo
├── Esflowix_back/ ← Repo del backend (Spring Boot)
└── Esflowix_front/ ← Repo del frontend (Angular)


## Contenedores

- `xlflow_db` → MySQL 8.0 (puerto 3308)
- `xlflow_backend` → Spring Boot (puerto 8083)
- `xlflow_frontend` → Angular + Nginx (puerto 8084)

## Deploy

```bash
# Clonar el repo de deploy
git clone <repo-deploy> /opt/xlflow
cd /opt/xlflow

# Clonar los subrepos
git clone <repo-back> Esflowix_back
git clone <repo-front> Esflowix_front

# Crear .env
cp .env.example .env
nano .env

# Levantar
docker compose up -d --build

cd /opt/xlflow

# Actualizar el compose
git pull

# Actualizar backend
cd Esflowix_back && git pull && cd ..

# Actualizar frontend
cd Esflowix_front && git pull && cd ..

# Rebuild y restart
docker compose up -d --build

# Ver estado
docker compose ps

# Ver logs
docker compose logs -f

# Ver logs de un servicio
docker compose logs -f backend

# Detener todos los container
docker compose down

# NO HACERLO - Detiene y borra volúmenes (¡CUIDADO! Borra la BD)
docker compose down -v

# Rebuild de un servicio (solo ese servicio)
docker compose up -d --build backend

Puertos
Servicio	Puerto host	    Puerto contenedor
MySQL	    3308	        3306
Backend	    8083	        8080
Frontend	8084	        80

Stack
Backend: Java 17, Spring Boot 3.3.5, JPA/Hibernate, MySQL, Apache POI

Frontend: Angular 19, Node 20

Infraestructura: Docker, Docker Compose, Nginx, MySQL 8.0