# n8n Docker Deployment

A production-ready n8n deployment using Docker Compose with Traefik reverse proxy, PostgreSQL database, and Redis cache.

## Features

- **n8n**: Latest version with workflow automation
- **PostgreSQL 15**: Reliable database backend
- **Redis 7**: High-performance caching
- **Traefik v3**: Reverse proxy with automatic SSL certificates
- **Let's Encrypt**: Automatic HTTPS certificates
- **Docker Compose**: Easy orchestration and management

## Prerequisites

- Docker and Docker Compose installed on the server
- A domain name pointing to your server
- Port 80 and 443 available

## Quick Start

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd n8n
   ```

2. **Copy the environment template**
   ```bash
   cp .env.example .env
   ```

3. **Edit the environment variables**
   ```bash
   nano .env
   ```
   Fill in your domain, database credentials, and n8n authentication details. You can choose any secure passwords for `POSTGRES_PASSWORD` and `N8N_BASIC_AUTH_PASSWORD`.

4. **Upload files to your remote server**
   Copy the `docker-compose.yml` and `.env` files to your remote server before starting the services.

5. **Start the services**
   ```bash
   docker compose up -d
   ```

6. **Access n8n**
   Open your browser and navigate to `https://your-domain.com`

## Environment Variables

The following environment variables need to be configured in your `.env` file:

- `N8N_DOMAIN`: Your n8n domain (e.g., n8n.example.com)
- `ACME_EMAIL`: Email for Let's Encrypt certificates
- `POSTGRES_DB`: PostgreSQL database name
- `POSTGRES_USER`: PostgreSQL username
- `POSTGRES_PASSWORD`: PostgreSQL password
- `N8N_BASIC_AUTH_USER`: n8n admin username
- `N8N_BASIC_AUTH_PASSWORD`: n8n admin password

## Services

### n8n
- **Image**: `n8nio/n8n:latest`
- **Port**: 5678 (internal)
- **Database**: PostgreSQL
- **Cache**: Redis
- **Authentication**: Basic Auth

### PostgreSQL
- **Image**: `postgres:15`
- **Port**: 5432 (internal)
- **Health Check**: Enabled
- **Persistent Storage**: Docker volume

### Redis
- **Image**: `redis:7-alpine`
- **Port**: 6379 (internal)
- **Health Check**: Enabled
- **Persistent Storage**: Docker volume

### Traefik
- **Image**: `traefik:v3.0`
- **Ports**: 80, 443
- **SSL**: Let's Encrypt automatic certificates
- **Dashboard**: Enabled

## Management Commands

```bash
# Start all services
docker compose up -d

# Stop all services
docker compose down

# View logs
docker compose logs -f

# View specific service logs
docker compose logs -f n8n

# Check service status
docker compose ps

# Update services
docker compose pull
docker compose up -d
```

## SSL Certificates

SSL certificates are automatically generated and renewed by Let's Encrypt through Traefik. Make sure your domain is properly configured to point to your server.

## Data Persistence

All data is stored in Docker volumes:
- `n8n-data`: n8n workflows and settings
- `postgres-data`: Database data
- `redis-data`: Cache data
- `traefik-letsencrypt`: SSL certificates

## Backup

To backup your n8n data:

```bash
# Backup n8n data
docker run --rm -v n8n-data:/data -v $(pwd):/backup alpine tar czf /backup/n8n-backup.tar.gz -C /data .

# Backup PostgreSQL
docker compose exec postgres pg_dump -U $POSTGRES_USER $POSTGRES_DB > backup.sql
```

## Security

- Basic authentication is enabled by default
- All traffic is encrypted with SSL
- Database and Redis are not exposed externally
- Regular security updates recommended

## Troubleshooting

1. **Check service status**: `docker compose ps`
2. **View logs**: `docker compose logs -f [service-name]`
3. **Verify environment variables**: `docker compose config`
4. **Check DNS**: Ensure your domain points to the server
5. **Firewall**: Ensure ports 80 and 443 are open

## Contributing

Feel free to submit issues and enhancement requests!

## License

This project is open source and available under the [MIT License](LICENSE).

---

*For a more entertaining version of this deployment story, check out [DEPLOYMENT_STORY.md](DEPLOYMENT_STORY.md)! 📚*
