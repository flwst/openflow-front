# 🚀 OpenFlow - Deployment Guide

## Quick Deploy (Same as Local)

OpenFlow works the same in production as in local development. No additional configuration needed!

### 1. Clone Repository

```bash
git clone <your-repo-url>
cd openflow-front
```

### 2. Configure Environment (Optional)

```bash
cd deployment/docker_compose
cp env.template .env
```

**Default works out of the box!** Only edit `.env` if you need to:
- Change ports
- Add custom domain  
- Configure external database

### 3. Deploy

```bash
# Same command as local!
docker-compose up -d
```

That's it! 🎉

### Accessing Services

- **Frontend**: `http://your-server:3000`
- **API**: `http://your-server:8080` (internal)
- **MCP Server**: `http://your-server:8000`

---

## What's Included

The deployment automatically starts:

✅ **Web Server** (Next.js frontend)  
✅ **API Server** (Python backend)  
✅ **MCP Workflow Server** (Python)  
✅ **PostgreSQL** (Database)  
✅ **Redis** (Cache)  
✅ **Vespa** (Search engine)  
✅ **MinIO** (File storage)  
✅ **Nginx** (Reverse proxy)  

---

## Environment Files Available

The project includes these example files you can copy:

- `deployment/docker_compose/env.template` - Main configuration
- `web/.env.local.example` - Frontend specific configs

**These work as-is!** No changes needed to run.

---

## For Production: Change Defaults

Only if deploying to production, consider changing:

```bash
# In .env file
POSTGRES_PASSWORD=your-secure-password
AUTH_TYPE=basic  # or disabled for testing
WEB_DOMAIN=your-domain.com
```

---

## Updating MCP Server

### Development (Current Setup)
Changes to `mcp-servers/workflow-server/` are reflected immediately:

```bash
# Just edit the files, restart container
docker-compose restart workflow_mcp_server
```

### Production Mode
To build MCP server into image:

```bash
docker-compose -f docker-compose.yml -f docker-compose.mcp-prod.yml up -d
```

After code changes in production:
```bash
docker-compose build workflow_mcp_server
docker-compose up -d workflow_mcp_server
```

---

## Troubleshooting

### Check Services Status
```bash
docker-compose ps
```

### View Logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f web_server
docker-compose logs -f workflow_mcp_server
```

### Restart Everything
```bash
docker-compose down
docker-compose up -d
```

---

## Ports

Default ports (changeable in `.env`):

- `3000` - Frontend
- `80` - Nginx (alternative frontend access)
- `8000` - MCP Server
- `8080` - API (not exposed by default)
- `5432` - PostgreSQL (not exposed by default)

---

## That's It!

OpenFlow uses the same simple deployment for local and production. 
Just `docker-compose up -d` and you're running! 🚀