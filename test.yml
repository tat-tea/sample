version: '3.8'

services:
  jenkins:
    image: jenkins/jenkins:2.190.2
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_data:/var/jenkins_home
    networks:
      - net

  pgpool:
    image: bitnami/pgpool:latest
    environment:
      - PGPOOL_BACKEND_NODES=0:postgres:5432
      - PGPOOL_POSTGRES_USERNAME=postgres
      - PGPOOL_POSTGRES_PASSWORD=postgres_password
      - PGPOOL_ADMIN_USERNAME=admin
      - PGPOOL_ADMIN_PASSWORD=admin_password
    ports:
      - "9999:9999"
    depends_on:
      - postgres
    networks:
      - net

  postgres:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: postgres_password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - net

volumes:
  jenkins_data:
  postgres_data:

networks:
  net:
