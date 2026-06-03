# Falling Tree Risk MCP Server

A remote [Model Context Protocol (MCP)](https://modelcontextprotocol.io) server that provides vegetation risk intelligence for any US property address. Assess tree-fall exposure using satellite canopy data, NOAA storm history, soil analysis, and calibrated probability scoring.

## Tools

### `get_falling_tree_risk`

Returns a complete vegetation risk profile for a given US property address:

- **Risk level** — Very Low, Low, Medium, High, or Very High
- **Annual probability range** — calibrated low/high bounds for tree fall on the property
- **Peak wind speed** — highest recorded wind speed in the last 2 years (km/h)
- **Max 24-hour rainfall** — heaviest 24-hour precipitation in the last 2 years (mm)
- **Ice storms per year** — average annual ice storm frequency
- **Vegetation density** — satellite-derived canopy density (Sparse, Moderate, Dense)
- **Tree health trend** — NDVI-based health trajectory (improving, stable, declining)
- **Roofline canopy overhang** — percentage of roof area under tree canopy
- **Soil type** — USDA soil series name
- **Soil drainage class** — drainage rating affecting root stability
- **Soil uprooting risk** — overall uprooting risk from soil conditions (Low, Moderate, High)

**Input:**

```json
{
  "address": "3024 251st Ave SE, Sammamish, WA 98075"
}
```

**Response:**

```json
{
  "address": "3024 251st Ave SE, Sammamish, WA 98075, USA",
  "latitude": 47.5912,
  "longitude": -122.0354,
  "risk_level": "Low",
  "property_risk_annual_low": 0.0047,
  "property_risk_annual_high": 0.0233,
  "ice_storms_per_year": 0.5,
  "max_wind_speed_kmh": 65.5,
  "max_precipitation_24h_mm": 45.2,
  "vegetation_density": "Dense",
  "tree_health_trend": "stable",
  "roofline_overhang_pct": 8.2,
  "soil_type": "Alderwood",
  "soil_drainage_class": "Moderately well drained",
  "soil_tree_risk": "Low"
}
```

## Setup

### 1. Get an API key

Sign up at [fallingtreerisk.com/api-access](https://www.fallingtreerisk.com/api-access) to get your API key.

### 2. Configure your MCP client

#### Claude Desktop / Claude Code

Add to your MCP configuration:

```json
{
  "mcpServers": {
    "fallingtreerisk": {
      "type": "streamable-http",
      "url": "https://mcp.fallingtreerisk.com",
      "headers": {
        "X-API-Key": "YOUR_API_KEY"
      }
    }
  }
}
```

#### Cursor / Other MCP clients

Use the remote server URL `https://mcp.fallingtreerisk.com` with your API key in the `X-API-Key` header.

## Use Cases

- **Insurance underwriting** — screen properties for tree-fall exposure before quoting
- **Real estate due diligence** — assess vegetation risk for buyers and agents
- **Property management** — identify high-risk trees near structures
- **AI assistants** — give LLMs access to property-level vegetation risk data

## Pricing

| Tier | Price |
|------|-------|
| Pay-as-you-go | $1.00 per unique property |
| Monthly | $100/month for up to 100 properties |

Re-queries on the same address within a billing cycle are free. Manage billing at [fallingtreerisk.com/api-access](https://www.fallingtreerisk.com/api-access).

## Rate Limits

| Limit | Value |
|-------|-------|
| Burst | 20 concurrent requests |
| Sustained rate | 10 requests/sec |
| Daily quota | 1,000 requests |

## Error Handling

The server uses standard [JSON-RPC 2.0](https://www.jsonrpc.org/specification) error codes:

| Code | Meaning | Description |
|------|---------|-------------|
| -32601 | Method Not Found | Unknown MCP method or tool |
| -32602 | Invalid Params | Missing or invalid address |
| -32603 | Internal Error | Server-side failure or weather data still loading (retry in 10-15s) |

## Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| `API key is required` | Missing X-API-Key header | Add your API key to the request headers |
| `Invalid API key` | Key is wrong or deactivated | Verify at [fallingtreerisk.com/api-access](https://www.fallingtreerisk.com/api-access) |
| `Weather data is being fetched` | First query for this location | Retry in 10-15 seconds |
| No response / timeout | Network issue | Verify connectivity to `https://mcp.fallingtreerisk.com` |

## Links

- [Website](https://www.fallingtreerisk.com)
- [API Access & Keys](https://www.fallingtreerisk.com/api-access)
- [Pricing](https://www.fallingtreerisk.com/subscription)

## License

Proprietary. See [fallingtreerisk.com](https://www.fallingtreerisk.com) for terms of service.
