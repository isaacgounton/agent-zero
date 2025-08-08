# Coolify Deployment Guide for Agent Zero

This guide explains how to deploy Agent Zero on Coolify using the provided Docker Compose configuration.

## Prerequisites

- Coolify instance running
- Domain name configured
- SSL certificate (Let's Encrypt recommended)

## Deployment Steps

### 1. Prepare Environment Variables

1. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```

2. Edit `.env` with your actual values:
   - Add your API keys (OpenAI, Anthropic, etc.)
   - Set secure passwords for UI_PASSWORD and ROOT_PASSWORD
   - Configure your domain in ALLOWED_HOSTS

### 2. Create Data Directory

```bash
sudo mkdir -p /opt/agent-zero/data
sudo chown 1000:1000 /opt/agent-zero/data
```

### 3. Deploy to Coolify

1. **Create New Resource** in Coolify
2. **Select Docker Compose**
3. **Upload** the `docker-compose.yml` file
4. **Configure Environment Variables**:
   - Import from your `.env` file
   - Or manually add each variable in Coolify's interface

### 4. Network Configuration

- **Port**: The service runs on port 3000 internally
- **Domain**: Configure your domain in Coolify
- **SSL**: Enable automatic SSL certificate generation

### 5. Health Checks

The compose file includes health checks for both services:
- Agent Zero: HTTP check on `/health` endpoint
- Redis: Redis ping command

## Security Features Implemented

### Container Security
- ✅ Non-root user execution (UID 1000)
- ✅ Read-only filesystem where possible
- ✅ Dropped unnecessary capabilities
- ✅ Security options enabled
- ✅ Resource limits configured

### Network Security
- ✅ Isolated Docker network
- ✅ No unnecessary port exposure
- ✅ Subnet configuration for network isolation

### Data Security
- ✅ Named volumes for persistent data
- ✅ Separate volumes for different data types
- ✅ Read-only configuration mounting
- ✅ Structured logging with rotation

### Application Security
- ✅ Authentication required (username/password)
- ✅ Session timeout configuration
- ✅ Secure cookie settings
- ✅ Allowed hosts configuration

## Monitoring and Maintenance

### Health Monitoring
- Health checks are configured for both services
- Coolify will automatically restart unhealthy containers
- Check logs in Coolify's interface for issues

### Log Management
- Logs are rotated automatically (max 100MB, 3 files)
- Access logs through Coolify's log viewer
- Consider setting up log aggregation for production

### Backup Strategy
- Agent Zero has built-in backup/restore functionality
- Use Coolify's backup features for volumes
- Backup environment variables securely

### Updates
- Use Coolify's update mechanism
- Data persists through updates via named volumes
- Test updates in staging environment first

## Troubleshooting

### Common Issues

1. **Permission Errors**:
   ```bash
   sudo chown -R 1000:1000 /opt/agent-zero/data
   ```

2. **Memory Issues**:
   - Increase resource limits in docker-compose.yml
   - Monitor memory usage in Coolify

3. **API Key Issues**:
   - Verify environment variables are set correctly
   - Check Agent Zero logs for API errors

4. **Connection Issues**:
   - Verify domain configuration
   - Check SSL certificate status
   - Ensure firewall allows traffic

### Health Check Failures
- Check if the application started properly
- Verify port 80 is accessible inside container
- Review application logs for startup errors

## Production Recommendations

1. **Use Strong Passwords**: Generate secure passwords for all authentication
2. **Enable 2FA**: If available in Agent Zero's future versions
3. **Regular Backups**: Set up automated backup schedules
4. **Monitor Resources**: Set up alerts for high CPU/memory usage
5. **Update Regularly**: Keep Agent Zero and dependencies updated
6. **Network Segmentation**: Consider VPN access for sensitive environments

## Support

For issues specific to:
- **Agent Zero**: Check the official documentation and GitHub issues
- **Coolify**: Refer to Coolify documentation and community
- **This Configuration**: Review the security settings and logs