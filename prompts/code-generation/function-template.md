# Function/Method Generation Prompt

Use this template when you need a specific function or method implemented.

## Prompt Template

```
Please write a [LANGUAGE] function that [PURPOSE].

## Function Specification
- **Name**: [function_name]
- **Parameters**: 
  - `param1`: [type] - [description]
  - `param2`: [type] - [description]
- **Return Type**: [type]
- **Returns**: [description of return value]

## Requirements
1. [Requirement 1]
2. [Requirement 2]
3. [Requirement 3]

## Behavior
- [Describe expected behavior in different scenarios]
- **Edge Cases**: [List edge cases to handle]
- **Error Handling**: [How to handle errors]

## Constraints
- Time Complexity: [e.g., O(n)]
- Space Complexity: [e.g., O(1)]
- [Any other constraints]

## Example Usage
```[language]
[Show example calls and expected outputs]
```
```

## Example Usage

```
Please write a Python function that validates and parses email addresses.

## Function Specification
- **Name**: parse_email
- **Parameters**: 
  - `email`: str - The email address to parse
  - `strict`: bool - If True, enforce stricter validation rules (default: False)
- **Return Type**: dict or None
- **Returns**: Dictionary with 'username' and 'domain' keys if valid, None if invalid

## Requirements
1. Validate email format according to RFC 5322 (basic validation)
2. Extract username and domain parts
3. Handle edge cases like multiple @ symbols
4. Support international domain names (IDN)

## Behavior
- Return None for obviously invalid emails
- In strict mode, reject emails with special characters in username
- Strip whitespace before validation
- **Edge Cases**: 
  - Empty string
  - Multiple @ symbols
  - Missing domain or username
  - Spaces in email
- **Error Handling**: Return None for invalid inputs, don't raise exceptions

## Constraints
- Time Complexity: O(n) where n is email length
- Space Complexity: O(1) for validation
- No external libraries (use standard library only)
- Must work with Python 3.8+

## Example Usage
```python
parse_email("user@example.com")
# Returns: {"username": "user", "domain": "example.com"}

parse_email("invalid.email")
# Returns: None

parse_email("user+tag@example.com", strict=False)
# Returns: {"username": "user+tag", "domain": "example.com"}

parse_email("user+tag@example.com", strict=True)
# Returns: None (special characters rejected in strict mode)
```
```

## Tips

- **Type Hints**: Specify expected types for parameters and return values
- **Complexity**: Mention if performance is critical
- **Dependencies**: State if external libraries are allowed or not
- **Version**: Specify language version if it matters
