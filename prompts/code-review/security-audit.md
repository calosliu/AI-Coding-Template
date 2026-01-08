# Security Audit Prompt

Use this template for security-focused code reviews.

## Prompt Template

```
Please perform a security audit on the following code.

## Code to Audit
```[language]
[Paste your code here]
```

## Security Checklist
Please check for:
- [ ] **Injection Attacks**: SQL, NoSQL, Command, LDAP injection
- [ ] **Authentication Issues**: Weak auth, session management
- [ ] **Authorization Issues**: Broken access control, privilege escalation
- [ ] **Data Exposure**: Sensitive data in logs, responses, errors
- [ ] **Cryptography**: Weak algorithms, poor key management
- [ ] **Input Validation**: Unvalidated/unsanitized user input
- [ ] **XSS/CSRF**: Cross-site scripting and request forgery
- [ ] **Dependencies**: Known vulnerable libraries
- [ ] **Configuration**: Hardcoded secrets, insecure defaults
- [ ] **Error Handling**: Information leakage in error messages

## Context
- **Application Type**: [web app, API, CLI, etc.]
- **Exposure**: [Public internet, Internal, Private]
- **Data Sensitivity**: [Public, Internal, Confidential, Restricted]
- **Compliance Requirements**: [GDPR, HIPAA, PCI-DSS, etc.]

## Threat Model (Optional)
- **Potential Attackers**: [Script kiddies, Competitors, Nation-state, etc.]
- **Attack Vectors**: [What attack methods are most concerning]
- **Assets at Risk**: [What needs protection]

## Output Format
For each vulnerability:
1. **Severity**: Critical / High / Medium / Low
2. **Vulnerability Type**: [e.g., SQL Injection, XSS]
3. **CWE Reference**: [Common Weakness Enumeration ID if applicable]
4. **Location**: [Line numbers or function names]
5. **Exploit Scenario**: [How an attacker could exploit this]
6. **Impact**: [What could happen if exploited]
7. **Fix**: [Detailed remediation steps]
8. **Secure Code Example**: [Show corrected code]
```

## Example Usage

```
Please perform a security audit on the following code.

## Code to Audit
```python
from flask import Flask, request, jsonify
import sqlite3

app = Flask(__name__)

@app.route('/api/user/<user_id>')
def get_user(user_id):
    conn = sqlite3.connect('app.db')
    cursor = conn.cursor()
    
    # Get user details
    query = f"SELECT id, username, email, role FROM users WHERE id = {user_id}"
    cursor.execute(query)
    user = cursor.fetchone()
    
    if user:
        return jsonify({
            'id': user[0],
            'username': user[1],
            'email': user[2],
            'role': user[3]
        })
    return jsonify({'error': 'User not found'}), 404

@app.route('/api/admin/users')
def list_users():
    # Get requesting user from session
    admin_user = request.cookies.get('user_id')
    
    conn = sqlite3.connect('app.db')
    cursor = conn.cursor()
    
    # Check if admin
    cursor.execute(f"SELECT role FROM users WHERE id = {admin_user}")
    result = cursor.fetchone()
    
    if result and result[0] == 'admin':
        cursor.execute("SELECT * FROM users")
        users = cursor.fetchall()
        return jsonify({'users': users})
    
    return jsonify({'error': 'Unauthorized'}), 403

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

## Security Checklist
Please check for:
- [x] **Injection Attacks**: SQL, NoSQL, Command, LDAP injection
- [x] **Authentication Issues**: Weak auth, session management
- [x] **Authorization Issues**: Broken access control, privilege escalation
- [x] **Data Exposure**: Sensitive data in logs, responses, errors
- [ ] **Cryptography**: Weak algorithms, poor key management
- [x] **Input Validation**: Unvalidated/unsanitized user input
- [ ] **XSS/CSRF**: Cross-site scripting and request forgery
- [ ] **Dependencies**: Known vulnerable libraries
- [x] **Configuration**: Hardcoded secrets, insecure defaults
- [x] **Error Handling**: Information leakage in error messages

## Context
- **Application Type**: REST API (Flask web application)
- **Exposure**: Public internet
- **Data Sensitivity**: Confidential (personal user data)
- **Compliance Requirements**: GDPR (EU users)

## Threat Model
- **Potential Attackers**: External hackers, malicious users
- **Attack Vectors**: SQL injection, broken authentication, privilege escalation
- **Assets at Risk**: User personal data, admin access, database integrity

## Output Format
For each vulnerability:
1. **Severity**: Critical / High / Medium / Low
2. **Vulnerability Type**: [e.g., SQL Injection, XSS]
3. **CWE Reference**: [Common Weakness Enumeration ID if applicable]
4. **Location**: [Line numbers or function names]
5. **Exploit Scenario**: [How an attacker could exploit this]
6. **Impact**: [What could happen if exploited]
7. **Fix**: [Detailed remediation steps]
8. **Secure Code Example**: [Show corrected code]
```

## Tips

- **Prioritize**: Focus on the most critical security concerns first
- **Context Matters**: Exposure level and data sensitivity affect severity
- **Be Thorough**: Include both common and subtle vulnerabilities
- **Actionable**: Request specific fixes, not just identification
- **Standards**: Reference OWASP Top 10, CWE, or other security standards
