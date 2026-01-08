# Test Data Generation Prompt

Use this template to generate realistic test data for your tests.

## Prompt Template

```
Please generate test data for [PURPOSE].

## Data Requirements
**Format**: [JSON, CSV, SQL, Python dict, etc.]
**Quantity**: [Number of records needed]
**Use Case**: [What tests will use this data]

## Schema/Structure
[Describe the data structure or provide schema]

## Data Characteristics
- **Realism**: [How realistic should it be?]
- **Variety**: [Need edge cases, normal cases, or both?]
- **Relationships**: [Any foreign keys or related data?]
- **Constraints**: [Unique values, ranges, formats]

## Specific Requirements
1. [Requirement 1]
2. [Requirement 2]
3. [Requirement 3]

## Edge Cases to Include
- [Edge case 1]
- [Edge case 2]

## Invalid Data (Optional)
Include some invalid data for negative testing:
- [Type of invalid data needed]

## Output Format
Provide data as:
- [Format specification]
- [Any grouping or organization]
```

## Example Usage

```
Please generate test data for testing an e-commerce order processing system.

## Data Requirements
**Format**: Python dictionary (suitable for PyTest fixtures)
**Quantity**: 
- 10 valid orders
- 5 edge case orders
- 3 invalid orders
**Use Case**: Testing order validation, payment processing, and fulfillment logic

## Schema/Structure
```python
{
    "order_id": "string (UUID)",
    "customer": {
        "id": "string (UUID)",
        "email": "string",
        "name": "string",
        "shipping_address": {
            "street": "string",
            "city": "string",
            "state": "string (2 letter code)",
            "zip": "string",
            "country": "string (2 letter code)"
        }
    },
    "items": [
        {
            "product_id": "string (UUID)",
            "name": "string",
            "quantity": "integer",
            "price": "float",
            "tax_rate": "float"
        }
    ],
    "payment": {
        "method": "string (credit_card, paypal, apple_pay)",
        "amount": "float",
        "currency": "string (3 letter code)",
        "status": "string (pending, completed, failed)"
    },
    "created_at": "string (ISO 8601)",
    "status": "string (pending, processing, shipped, delivered, cancelled)"
}
```

## Data Characteristics
- **Realism**: Should look like real orders (realistic names, addresses, prices)
- **Variety**: Mix of single-item and multi-item orders, various payment methods
- **Relationships**: Each order should have consistent totals (item prices + tax = payment amount)
- **Constraints**: 
  - order_id, customer.id, product_id must be valid UUIDs
  - Email addresses must be valid format
  - Prices must be positive, max 2 decimal places
  - Tax rates between 0 and 0.15 (0-15%)
  - Quantities between 1 and 100

## Specific Requirements
1. Include at least 2 orders with multiple items (2-5 items each)
2. Include orders from different countries (US, CA, UK, DE)
3. Include different payment methods (at least 2 of each type)
4. Make sure payment.amount matches sum of (item.price * item.quantity * (1 + item.tax_rate))
5. Use realistic product names (electronics, clothing, books, etc.)

## Edge Cases to Include
- Order with single item at minimum quantity (1)
- Order with single item at high quantity (99)
- Order with zero tax rate (tax-exempt)
- Order with maximum tax rate (15%)
- Order with total amount at exact round number ($100.00)
- Order from international address with special characters (ü, ñ, etc.)
- Very recent order (created_at within last hour)
- Order with very long product name (100+ characters)

## Invalid Data
Include some invalid data for negative testing:
- Order with negative quantity
- Order with invalid email format
- Order where payment amount doesn't match item totals
- Order with invalid UUID format
- Order with missing required fields

## Output Format
Provide data as:
- Python dictionary with three keys: "valid_orders", "edge_case_orders", "invalid_orders"
- Each order should be a separate dictionary in a list
- Include comments explaining what makes edge cases and invalid data special
- Format for easy copy-paste into a pytest fixture
```

## Tips

- **Realistic**: Use realistic values that mirror production data
- **Diverse**: Include variety to catch different bugs
- **Documented**: Comment on why certain test data exists
- **Reusable**: Structure for easy adaptation to other tests
- **Maintainable**: Use patterns that are easy to update
