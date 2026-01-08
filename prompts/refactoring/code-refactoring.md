# Code Refactoring Prompt

Use this template to improve existing code structure, readability, and maintainability.

## Prompt Template

```
Please refactor the following code to improve [ASPECT].

## Code to Refactor
```[language]
[Paste code here]
```

## Refactoring Goals
Please improve:
- [ ] **Readability**: Make code easier to understand
- [ ] **Maintainability**: Make code easier to modify
- [ ] **Performance**: Optimize for speed or resource usage
- [ ] **Testability**: Make code easier to test
- [ ] **Modularity**: Break into smaller, reusable components
- [ ] **Error Handling**: Improve error management
- [ ] **Code Duplication**: Remove repeated code (DRY principle)
- [ ] **Naming**: Improve variable/function names
- [ ] **Design Patterns**: Apply appropriate patterns

## Constraints
- **Preserve Behavior**: [Must maintain exact behavior / Can improve behavior]
- **Breaking Changes**: [Allowed / Not allowed]
- **Dependencies**: [Can add libraries / Must use existing]
- **Style Guide**: [Follow specific style guide if applicable]

## Context
- **Code Purpose**: [What this code does]
- **Current Issues**: [Known problems or pain points]
- **Future Plans**: [How code might evolve]

## Output Format
Please provide:
1. Refactored code with clear structure
2. Explanation of changes made
3. Before/after comparison for key improvements
4. Any new tests needed
5. Migration notes if breaking changes
```

## Example Usage

```
Please refactor the following code to improve readability, maintainability, and testability.

## Code to Refactor
```python
def process(data):
    # Process user data
    r = []
    for d in data:
        if d['type'] == 'user':
            if d['status'] == 'active':
                if 'email' in d and d['email']:
                    if '@' in d['email']:
                        age = 2024 - d['birth_year'] if 'birth_year' in d else 0
                        if age >= 18:
                            u = {
                                'id': d['id'],
                                'name': d['first_name'] + ' ' + d['last_name'],
                                'email': d['email'],
                                'age': age,
                                'score': (d['points'] * 1.5) if d.get('premium') else d['points']
                            }
                            r.append(u)
    return r

def send_emails(users):
    # Send emails to users
    c = 0
    f = 0
    for u in users:
        try:
            msg = f"Hello {u['name']}, your score is {u['score']}!"
            # Send email
            import smtplib
            s = smtplib.SMTP('smtp.gmail.com', 587)
            s.starttls()
            s.login('app@example.com', 'password123')
            s.sendmail('app@example.com', u['email'], msg)
            s.quit()
            c += 1
        except:
            f += 1
    print(f"Sent: {c}, Failed: {f}")
    return c, f

def main():
    import json
    with open('data.json') as f:
        data = json.load(f)
    users = process(data)
    send_emails(users)
```

## Refactoring Goals
Please improve:
- [x] **Readability**: Make code easier to understand
- [x] **Maintainability**: Make code easier to modify
- [ ] **Performance**: Not a priority
- [x] **Testability**: Make code easier to test
- [x] **Modularity**: Break into smaller, reusable components
- [x] **Error Handling**: Improve error management
- [x] **Code Duplication**: Remove repeated code (DRY principle)
- [x] **Naming**: Improve variable/function names
- [x] **Design Patterns**: Apply appropriate patterns

## Constraints
- **Preserve Behavior**: Must maintain exact behavior for valid inputs, can improve error handling
- **Breaking Changes**: Allowed (internal refactor, not public API)
- **Dependencies**: Can add standard library modules, prefer not to add external dependencies
- **Style Guide**: Follow PEP 8 and Python best practices

## Context
- **Code Purpose**: 
  - Filter active adult users from dataset
  - Calculate engagement scores
  - Send personalized emails
- **Current Issues**: 
  - Hard to test (no dependency injection)
  - Deep nesting makes it hard to read
  - Poor error handling (bare except, hardcoded credentials)
  - Single-letter variables are cryptic
  - Email connection created per user (inefficient)
- **Future Plans**: 
  - Might add more user filters
  - Need to support different email providers
  - Want to add unit tests
  - May need to send different types of emails

## Output Format
Please provide:
1. Refactored code with clear structure and good separation of concerns
2. Explanation of changes made and why
3. Before/after comparison highlighting key improvements
4. Suggestions for unit tests that should be added
5. Configuration file example for credentials (don't hardcode)
```

## Tips

- **Small Steps**: Refactor incrementally, test after each change
- **Tests First**: Ensure tests exist before refactoring
- **One Goal**: Focus on one aspect at a time
- **Preserve Behavior**: Don't change what the code does (unless that's the goal)
- **SOLID Principles**: Apply Single Responsibility, Open/Closed, etc.
