# Code Review Checklist Prompt

Use this template to get comprehensive code reviews from AI assistants.

## Prompt Template

```
Please review the following code and provide feedback.

## Code to Review
```[language]
[Paste your code here]
```

## Review Focus Areas
Please check for:
- [ ] **Correctness**: Logic errors, bugs, edge cases
- [ ] **Performance**: Inefficient algorithms, unnecessary operations
- [ ] **Security**: Vulnerabilities, injection risks, data exposure
- [ ] **Maintainability**: Code clarity, naming, structure
- [ ] **Best Practices**: Language/framework conventions
- [ ] **Testing**: Testability, missing test cases
- [ ] **Documentation**: Comments, docstrings, clarity

## Context
- **Purpose**: [What this code does]
- **Language/Framework**: [e.g., Python 3.11, React 18]
- **Related Files**: [Mention dependencies or related code]

## Specific Concerns (Optional)
[Any specific areas you're unsure about]

## Output Format
For each issue found, provide:
1. **Severity**: Critical / High / Medium / Low
2. **Category**: [e.g., Security, Performance, Bug]
3. **Location**: [Line numbers or function names]
4. **Issue**: [Description of the problem]
5. **Suggestion**: [How to fix it]
6. **Example**: [Show corrected code if applicable]
```

## Example Usage

```
Please review the following code and provide feedback.

## Code to Review
```python
def process_user_data(user_id):
    conn = sqlite3.connect('users.db')
    cursor = conn.cursor()
    query = f"SELECT * FROM users WHERE id = {user_id}"
    cursor.execute(query)
    result = cursor.fetchone()
    
    if result:
        user_data = {
            'name': result[1],
            'email': result[2],
            'password': result[3]
        }
        return json.dumps(user_data)
    return None
```

## Review Focus Areas
Please check for:
- [x] **Correctness**: Logic errors, bugs, edge cases
- [x] **Performance**: Inefficient algorithms, unnecessary operations
- [x] **Security**: Vulnerabilities, injection risks, data exposure
- [x] **Maintainability**: Code clarity, naming, structure
- [x] **Best Practices**: Language/framework conventions
- [ ] **Testing**: Testability, missing test cases
- [ ] **Documentation**: Comments, docstrings, clarity

## Context
- **Purpose**: Fetch user data from database and return as JSON
- **Language/Framework**: Python 3.11 with SQLite
- **Related Files**: Part of a Flask API endpoint

## Specific Concerns
I'm worried about security - is this safe to use in production?

## Output Format
For each issue found, provide:
1. **Severity**: Critical / High / Medium / Low
2. **Category**: [e.g., Security, Performance, Bug]
3. **Location**: [Line numbers or function names]
4. **Issue**: [Description of the problem]
5. **Suggestion**: [How to fix it]
6. **Example**: [Show corrected code if applicable]
```

## Tips

- **Be Specific**: Tell the AI what aspects matter most to you
- **Provide Context**: Explain what the code does and where it's used
- **Ask Questions**: If unsure about something specific, highlight it
- **Iterative**: You can ask follow-up questions on specific findings
