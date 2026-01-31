---
name: provider-pricing
category: providers
impact: HIGH
metadata:
  tags: providers, pricing, costs, api
---

# Pricing lookup (fal API)

Use the pricing endpoint to verify current rates before estimating costs.

## Copy page

Returns unit pricing for requested endpoint IDs. Most models use output-based pricing (e.g., per image/video with proportional adjustments for resolution/length). Some models use GPU-based pricing depending on architecture. Values are expressed per model's billing unit in a given currency.

Authentication: Required. Users must provide a valid API key. Custom pricing or discounts may be applied based on account status.

Common Use Cases:

- Display pricing in user interfaces
- Compare pricing across different models
- Build cost estimation tools
- Check current billing rates
- See fal.ai pricing for more details.

GET
/
models
/
pricing
