# web1 Configuration Files

This directory contains configuration files for web1.

## Files

### configuration.yaml
Main web1 configuration file.

**Important settings:**

- **HTTP Configuration**: Required for proper proxy operation
  ```yaml
  http:
    use_x_forwarded_for: true
    server_host: 0.0.0.0
    trusted_proxies:
      - 172.19.0.0/16  # Docker network for nginx proxy
      - 127.0.0.1
      - 100.64.0.0/10  # Tailscale network
  ```

  ⚠️ **Critical**: The `trusted_proxies` section must include the Docker network subnet (`172.19.0.0/16`) to allow nginx proxy to forward requests to web1. Without this, you will get **400 Bad Request** errors when accessing through the proxy.

### Other Files

- `automations.yaml` - Automation configurations (managed by WEB1 UI)
- `scripts.yaml` - Script configurations (managed by WEB1 UI)
- `scenes.yaml` - Scene configurations (managed by WEB1 UI)

## Deployment

To deploy configuration changes to the server:

1. Edit the configuration file locally
2. Copy to server:
   ```bash
   scp config/configuration.yaml web1-vpn:/opt/web1/web1/configuration.yaml
   ```
3. Restart web1:
   ```bash
   ssh web1-vpn "cd /opt/web1 && sudo docker compose restart web1"
   ```

## Notes

- Always backup configuration before making changes
- Test configuration changes on the server before committing
- Some configuration files (like `automations.yaml`) are managed by web1 UI and should not be edited manually
