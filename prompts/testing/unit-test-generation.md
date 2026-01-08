# Unit Test Generation Prompt

Use this template to generate unit tests for your code.

## Prompt Template

```
Please generate unit tests for the following code.

## Code to Test
```[language]
[Paste the code/function to test]
```

## Testing Requirements
- **Testing Framework**: [e.g., Jest, PyTest, JUnit, RSpec]
- **Coverage Goal**: [e.g., 100%, focus on critical paths]
- **Test Style**: [e.g., AAA pattern, BDD style, TDD]

## Test Scenarios
Please include tests for:
1. **Happy Path**: Normal, expected inputs
2. **Edge Cases**: Boundary conditions, empty inputs, max values
3. **Error Cases**: Invalid inputs, exceptions, error handling
4. **Integration Points**: Mocked dependencies, external calls

## Specific Test Cases (Optional)
[List any specific scenarios you want tested]

## Mocking Requirements
- Mock: [List what should be mocked]
- Real: [List what should use real implementations]

## Code Quality
- Include descriptive test names
- Use arrange-act-assert pattern
- Add comments for complex test logic
- Follow existing test conventions

## Output Format
Provide:
1. Complete test file with all imports
2. Setup/teardown code if needed
3. Mock configurations
4. Test data fixtures if applicable
```

## Example Usage

```
Please generate unit tests for the following code.

## Code to Test
```python
def calculate_shipping_cost(weight_kg, distance_km, express=False):
    """
    Calculate shipping cost based on weight and distance.
    
    Args:
        weight_kg: Package weight in kilograms (0.1 to 30)
        distance_km: Shipping distance in kilometers (1 to 5000)
        express: Whether to use express shipping (default: False)
    
    Returns:
        float: Shipping cost in dollars
        
    Raises:
        ValueError: If weight or distance is out of valid range
    """
    # Validate inputs
    if weight_kg < 0.1 or weight_kg > 30:
        raise ValueError("Weight must be between 0.1 and 30 kg")
    if distance_km < 1 or distance_km > 5000:
        raise ValueError("Distance must be between 1 and 5000 km")
    
    # Base rate: $2 per kg
    base_cost = weight_kg * 2.0
    
    # Distance multiplier
    if distance_km <= 100:
        distance_multiplier = 1.0
    elif distance_km <= 500:
        distance_multiplier = 1.5
    else:
        distance_multiplier = 2.0
    
    cost = base_cost * distance_multiplier
    
    # Express shipping adds 50%
    if express:
        cost *= 1.5
    
    # Minimum charge
    return max(cost, 5.0)
```

## Testing Requirements
- **Testing Framework**: PyTest
- **Coverage Goal**: 100% line and branch coverage
- **Test Style**: AAA (Arrange-Act-Assert) pattern

## Test Scenarios
Please include tests for:
1. **Happy Path**: 
   - Normal weight and distance
   - With and without express shipping
2. **Edge Cases**: 
   - Minimum weight (0.1 kg)
   - Maximum weight (30 kg)
   - Minimum distance (1 km)
   - Maximum distance (5000 km)
   - Different distance brackets (≤100, ≤500, >500)
   - Minimum charge threshold
3. **Error Cases**: 
   - Weight too low (< 0.1)
   - Weight too high (> 30)
   - Distance too low (< 1)
   - Distance too high (> 5000)
   - Invalid types (string, None, negative)

## Specific Test Cases
- Test that express shipping exactly multiplies cost by 1.5
- Test that minimum charge of $5 is enforced
- Test all distance bracket boundaries (100 km, 500 km)

## Mocking Requirements
- Mock: None (pure function)
- Real: Use real implementations

## Code Quality
- Include descriptive test names following pattern: test_<scenario>_<expected_outcome>
- Use arrange-act-assert pattern
- Add comments for complex assertions
- Use PyTest fixtures for common test data
- Use parametrize for similar test cases

## Output Format
Provide:
1. Complete test file with all imports
2. PyTest fixtures for test data
3. Parametrized tests where appropriate
4. Clear test organization by scenario type
```

## Tips

- **Comprehensive**: Cover all branches and edge cases
- **Isolated**: Each test should be independent
- **Fast**: Tests should run quickly
- **Deterministic**: Same input always gives same result
- **Readable**: Test names should describe what they test
