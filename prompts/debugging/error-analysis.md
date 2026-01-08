# Error Analysis Prompt

Use this template to analyze error messages and stack traces.

## Prompt Template

```
I need help understanding and fixing this error.

## Error Message
```
[Paste complete error message]
```

## Stack Trace
```
[Paste complete stack trace]
```

## Context
**What I was trying to do**: [Action that caused the error]
**Code that triggered it**:
```[language]
[Paste relevant code]
```

## Environment
- **Language/Framework**: [Version]
- **Operating System**: [OS and version]
- **Relevant Libraries**: [Versions]

## When Does It Occur?
- **Consistently**: [Every time / Sometimes / Rarely]
- **Conditions**: [What conditions trigger it]
- **Recent Changes**: [What changed before error appeared]

## Additional Logs
```
[Any additional relevant logs]
```

## Investigation Requests
Please help me:
1. Explain what the error means in plain language
2. Identify the root cause
3. Explain why it's happening
4. Provide step-by-step fix instructions
5. Suggest how to prevent it in the future
6. Recommend error handling strategies
```

## Example Usage

```
I need help understanding and fixing this error that crashes my application.

## Error Message
```
Traceback (most recent call last):
  File "/app/main.py", line 45, in process_order
    customer = Customer.objects.get(id=order_data['customer_id'])
  File "/usr/local/lib/python3.11/site-packages/django/db/models/manager.py", line 85, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "/usr/local/lib/python3.11/site-packages/django/db/models/query.py", line 633, in get
    raise self.model.DoesNotExist(
Customer.DoesNotExist: Customer matching query does not exist.

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/app/main.py", line 52, in process_order
    logger.error(f"Customer not found: {customer.id}")
UnboundLocalError: local variable 'customer' referenced before assignment
```

## Stack Trace
```
Full traceback:
  File "/app/api/views.py", line 123, in create_order
    result = process_order(request.data)
  File "/app/main.py", line 45, in process_order
    customer = Customer.objects.get(id=order_data['customer_id'])
  [... Django internals ...]
  File "/app/main.py", line 52, in process_order
    logger.error(f"Customer not found: {customer.id}")
UnboundLocalError: local variable 'customer' referenced before assignment
```

## Context
**What I was trying to do**: Process a new order submission from the API
**Code that triggered it**:
```python
def process_order(order_data):
    """Process a new order and create order record"""
    
    try:
        # Get customer
        customer = Customer.objects.get(id=order_data['customer_id'])
        
        # Validate inventory
        for item in order_data['items']:
            product = Product.objects.get(id=item['product_id'])
            if product.stock < item['quantity']:
                raise ValueError(f"Insufficient stock for {product.name}")
        
        # Create order
        order = Order.objects.create(
            customer=customer,
            total=calculate_total(order_data['items']),
            status='pending'
        )
        
        # Add order items
        for item in order_data['items']:
            OrderItem.objects.create(
                order=order,
                product_id=item['product_id'],
                quantity=item['quantity'],
                price=item['price']
            )
        
        return order
        
    except Customer.DoesNotExist:
        logger.error(f"Customer not found: {customer.id}")
        raise ValueError("Invalid customer ID")
    except Product.DoesNotExist:
        logger.error(f"Product not found: {product.id}")
        raise ValueError("Invalid product ID")
    except Exception as e:
        logger.error(f"Order processing failed: {str(e)}")
        raise
```

## Environment
- **Language/Framework**: Python 3.11, Django 4.2
- **Operating System**: Ubuntu 22.04 (in Docker container)
- **Relevant Libraries**: 
  - Django 4.2.7
  - psycopg2 2.9.9 (PostgreSQL driver)

## When Does It Occur?
- **Consistently**: Every time a customer ID that doesn't exist is submitted
- **Conditions**: 
  - Happens when frontend sends invalid customer_id
  - Test data sometimes has customer IDs that were deleted
- **Recent Changes**: 
  - Just added the error logging in exception handlers
  - Worked before adding the logging statements

## Additional Logs
```
[2024-01-15 10:23:45] INFO: Processing order for customer_id=999
[2024-01-15 10:23:45] ERROR: Customer matching query does not exist.
[2024-01-15 10:23:45] ERROR: UnboundLocalError: local variable 'customer' referenced before assignment
[2024-01-15 10:23:45] ERROR: Request failed with 500 Internal Server Error
```

## Investigation Requests
Please help me:
1. Explain what the error means (why is 'customer' unbound?)
2. Identify the root cause (why does this happen in the exception handler?)
3. Explain why it's happening (Python scoping or exception handling issue?)
4. Provide step-by-step fix instructions
5. Suggest how to prevent similar errors (better exception handling patterns)
6. Recommend error handling strategies for Django views

## Additional Questions
- Is there a better way to handle Django DoesNotExist exceptions?
- Should I be using get_object_or_404() instead?
- How can I safely log information about objects that might not exist?
```

## Tips

- **Complete**: Include full error messages and stack traces
- **Context**: Show the code that caused the error
- **Environment**: Version numbers matter for error diagnosis
- **Reproducibility**: Explain how to reproduce the error
- **Attempts**: Mention what you've already tried
