# cachet-monitor image

Usage :

```bash
docker run -tid -v /path/to/config:/etc herveleclerc/cachet-monitor  -c /etc/cachet-monitor.config.json
```

in /path/to/config create a file ```cachet-monitor.config.json```

Example of configuration :

```
{
  // URL for the API. Note: Must end with /api/v1
  "api_url": "https://<cachet domain>/api/v1",
  // Your API token for Cachet
  "api_token": "<cachet api token>",
  // optional, false default, set if your certificate is self-signed/untrusted
  "insecure_api": false,
  "monitors": [{
    // required, friendly name for your monitor
    "name": "Name of your monitor",
    // required, url to probe
    "url": "Ping URL",
    // optional, http method (defaults GET)
    "method": "get",
    // self-signed ssl certificate
    "strict_tls": true,
    // seconds between checks
    "interval": 10,
    // seconds for http timeout
    "timeout": 5,
    // post lag to cachet metric (graph)
    // note either metric ID or component ID are required
    "metric_id": <metric id>,
    // post incidents to this component
    "component_id": <component id>,
    // If % of downtime is over this threshold, open an incident
    "threshold": 80,
    // optional, expected status code (either status code or body must be supplied)
    "expected_status_code": 200,
    // optional, regular expression to match body content
    "expected_body": "P.*NG"
  }],
  // optional, system name to identify bot (uses hostname by default)
  "system_name": "",
  // optional, defaults to stdout
  "log_path": ""
}

```


Other arguments available :

```
  -c="/etc/cachet-monitor.config.json": Config path
  -log="": Log path
  -name="": System Name
```

An example of ```docker-compose file```

```yaml
version: "2"
services:
  cachet-monitor-portal:
    image: herveleclerc/docker-cachet-monitor
    container_name: cachet-monitor-portal
    restart: always
    volumes:
      - /etc/docker/compose/cachet-monitor:/etc
    command: -c /etc/cachet-monitor.config.json
```

