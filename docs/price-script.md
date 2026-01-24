Script to include:
<script id="effic_price_service" src="https://try.noobru.com/external-scripts/effic_price_service.js" defer></script>


Variant IDS

1 Month: 46898870943989
3 Months: 46898870976757
6 Months 46898871009525

Please note that 1 month of Noobru consists of 20 sachets.


# Price Exchange Script Documentation

## Overview

This script (`effic_price_service`) automatically retrieve prices from our Shopify shop and exchanges it to the customer's local currency. It supports both dynamic variant-based pricing and static pricing like a fixed shipping price or similar.

## How It Works

1. On page load, the script:
   - Detects the customer's country and currency
   - Fetches the current exchange rate
   - Automatically exchanges all prices on the page

2. The script listens for custom events to trigger price exchanges:
   - `effic:exchange_prices` - Exchanges dynamic variant prices
   - `effic:exchange_static_prices` - Exchanges static prices

## Configuration Variables

Before using the script, you can configure these variables:

- `enable_auto_discount` - Enable automatic coupon code detection from URL (default: `false`)

## Dynamic Variant Pricing

Use this for prices that come from Shopify product variants.

### Basic Usage

Add the `data-variant` attribute to any element containing a price:

```html
<span data-variant="123456789">£10.00</span>
```

The script will:
1. Fetch the price for variant ID `123456789`
2. Exchange it to the customer's currency
3. Update the element's content with the formatted price

### Data Attributes for Dynamic Pricing

#### `data-variant` (Required)
- **Type**: String/Number
- **Description**: The Shopify variant ID
- **Example**: `data-variant="123456789"`

#### `data-compare_at` or `data-compare_at_price`
- **Type**: Boolean (presence indicates true)
- **Description**: When present, uses the compare-at price instead of the regular price
- **Example**: 
  ```html
  <span data-variant="123456789" data-compare_at>£15.00</span>
  ```

#### `data-perUnit` or `data-per_unit`
- **Type**: Number
- **Description**: Divides the price by this number to show per-unit pricing
- **Example**: 
  ```html
  <span data-variant="123456789" data-per-unit="12">£120.00</span>
  <!-- Will display as price per unit (price / 12) -->
  ```

#### `data-subtract_from`
- **Type**: String
- **Description**: Subtracts the regular price from another price field
- **Valid values**: `'compare_at'` or `'compare_at_price'`
- **Example**: 
  ```html
  <span data-variant="123456789" data-subtract_from="compare_at_price">
    <!-- Will show: compare_at_price - regular_price -->
  </span>
  ```

#### `data-force_exchange`
- **Type**: Boolean (presence indicates true)
- **Description**: Forces the price to be re-exchanged even if it was already exchanged to the current currency
- **Use case**: Useful when currency changes dynamically or when you need to refresh prices
- **Example**: 
  ```html
  <span data-variant="123456789" data-force_exchange>£10.00</span>
  ```

#### `data-exchangedTo` (Auto-generated)
- **Type**: String
- **Description**: Automatically set by the script to track which currency the price was exchanged to
- **Note**: You don't need to set this manually - it's used internally to prevent duplicate exchanges

## Static Pricing

Use this for fixed prices that don't come from Shopify variants.

### Basic Usage

Add the `data-exchange` attribute with the price in minor units (cents/pence):

```html
<span data-exchange="1000">£10.00</span>
<!-- 1000 = £10.00 (in minor units) -->
```

### Data Attributes for Static Pricing

#### `data-exchange` (Required)
- **Type**: Number
- **Description**: The price in minor units (e.g., cents, pence). For £10.00, use `1000`
- **Example**: 
  ```html
  <span data-exchange="1999">£19.99</span>
  ```

#### `data-quantity`
- **Type**: Number
- **Description**: Multiplies the price by this quantity (default: `1`)
- **Example**: 
  ```html
  <span data-exchange="1000" data-quantity="3">£30.00</span>
  <!-- Will calculate: (1000 * 3) / 100 = £30.00 -->
  ```

#### `data-force_exchange`
- **Type**: Boolean (presence indicates true)
- **Description**: Forces the price to be re-exchanged even if already exchanged
- **Example**: 
  ```html
  <span data-exchange="1000" data-force_exchange>£10.00</span>
  ```

## Examples

### Example 1: Simple Variant Price

```html
<!-- Original price in GBP -->
<span data-variant="123456789">£10.00</span>
<!-- Will automatically exchange to customer's currency -->
```

### Example 2: Compare At Price

```html
<!-- Shows the compare-at price instead of regular price -->
<span data-variant="123456789" data-compare_at>£15.00</span>
```

### Example 3: Per-Unit Pricing

```html
<!-- Shows price per unit (total price / 12) -->
<span data-variant="123456789" data-per-unit="12">£120.00</span>
```

### Example 4: Discount Display (Compare At - Regular)

```html
<!-- Shows the discount amount -->
<span data-variant="123456789" data-subtract_from="compare_at_price">
  Save £5.00
</span>
```

### Example 5: Static Price with Quantity

```html
<!-- Static price for 3 items -->
<span data-exchange="1000" data-quantity="3">£30.00</span>
```

### Example 6: Force Re-exchange

```html
<!-- Force price to update when currency changes -->
<span data-variant="123456789" data-force_exchange>£10.00</span>
```

## Programmatic Usage

### Trigger Price Exchange Manually

You can trigger price exchanges programmatically by dispatching custom events:

```javascript
// Exchange all variant prices
window.dispatchEvent(new Event('effic:exchange_prices'));

// Exchange all static prices
window.dispatchEvent(new Event('effic:exchange_static_prices'));
```

### Coupon Code Support

If `enable_auto_discount` is set to `true`, the script automatically detects coupon codes from the URL:

```
https://yoursite.com?coupon=SUMMER2024
```

The coupon code will be automatically included in price API requests.

## Currency Formatting

The script automatically formats prices according to:
- The customer's detected country
- The appropriate currency symbol
- Local number formatting conventions

Special handling:
- **EUR**: Uses € symbol (removes "EUR" text)
- **USD**: Uses $ symbol (removes "US$" text)
- **JPY**: Prices are NOT divided by 100 (already in major units)
- Other currencies: Standard locale formatting

## API Endpoints

The script uses these endpoints:

1. **Get Location Info**: `GET /get-li`
   - Returns: `{ country: 'GB', ... }`

2. **Get Exchange Rate**: `GET /api/rate?from=GBP&to=USD`
   - Returns: Exchange rate number

3. **Get Variant Price**: `GET /api/variant-prices?variant_id=123456789&coupon=CODE`
   - Returns: `{ price: 1000, compare_at_price: 1500, currency: 'GBP', country: 'GB' }`

## Caching

The script implements intelligent caching:
- Exchange rates are cached per currency
- Variant prices are cached per variant ID and currency
- Prevents duplicate API calls for the same variant/currency combination

## Notes

- Prices are stored in **minor units** (cents/pence) in the API
- The script automatically converts to major units (dollars/pounds) except for JPY
- The script runs automatically on `DOMContentLoaded`
- Multiple elements with the same `data-variant` will share the same cached price data
