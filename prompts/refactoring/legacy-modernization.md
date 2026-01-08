# Legacy Code Modernization Prompt

Use this template to modernize old code to use current best practices and language features.

## Prompt Template

```
Please modernize the following legacy code to use current best practices.

## Legacy Code
```[language]
[Paste old code here]
```

## Current State
- **Original Language Version**: [e.g., Python 2.7, Java 8, ES5]
- **Target Language Version**: [e.g., Python 3.11, Java 17, ES2023]
- **Age**: [How old is this code]
- **Last Modified**: [When it was last updated]

## Modernization Goals
Update to use:
- [ ] Modern language features (list specific features)
- [ ] Current standard library APIs
- [ ] Type hints/annotations
- [ ] Modern error handling
- [ ] Async/await (if applicable)
- [ ] Current security best practices
- [ ] Modern testing approaches
- [ ] Updated dependencies
- [ ] Current coding conventions

## Known Issues
- [Issue 1 with current code]
- [Issue 2 with current code]

## Constraints
- **Backward Compatibility**: [Must maintain / Can break]
- **Testing**: [Existing tests to preserve / Need new tests]
- **Deployment**: [Gradual rollout / Full replacement]
- **Dependencies**: [Can update freely / Must keep compatible]

## Context
- **Production Usage**: [How critical is this code]
- **Change Frequency**: [Often modified / Rarely touched]
- **Technical Debt**: [Known issues to address]

## Output Format
Provide:
1. Modernized code with explanatory comments
2. List of changes made and why
3. Breaking changes (if any) and migration guide
4. Updated tests
5. Dependency updates needed
6. Deprecation warnings for removed features
```

## Example Usage

```
Please modernize the following legacy code to use current best practices.

## Legacy Code
```python
# Python 2.7 code written in 2015
import urllib2
import json
from datetime import datetime

class UserManager:
    def __init__(self, api_url, api_key):
        self.api_url = api_url
        self.api_key = api_key
    
    def get_user(self, user_id):
        url = "%s/users/%s" % (self.api_url, user_id)
        request = urllib2.Request(url)
        request.add_header('X-API-Key', self.api_key)
        
        try:
            response = urllib2.urlopen(request)
            data = response.read()
            user_dict = json.loads(data)
            return user_dict
        except urllib2.HTTPError, e:
            print "Error fetching user:", e.code
            return None
        except Exception, e:
            print "Unexpected error:", str(e)
            return None
    
    def get_users(self, filters={}):
        url = self.api_url + "/users"
        
        # Build query string
        if filters:
            query_parts = []
            for key, value in filters.iteritems():
                query_parts.append("%s=%s" % (key, value))
            url += "?" + "&".join(query_parts)
        
        request = urllib2.Request(url)
        request.add_header('X-API-Key', self.api_key)
        
        response = urllib2.urlopen(request)
        data = json.loads(response.read())
        return data
    
    def create_user(self, user_data):
        url = self.api_url + "/users"
        
        request = urllib2.Request(url)
        request.add_header('X-API-Key', self.api_key)
        request.add_header('Content-Type', 'application/json')
        request.add_data(json.dumps(user_data))
        
        response = urllib2.urlopen(request)
        return json.loads(response.read())
    
    def format_user(self, user):
        # Format user for display
        name = "%s %s" % (user.get('first_name', ''), user.get('last_name', ''))
        email = user.get('email', 'N/A')
        
        # Calculate age
        if user.has_key('birth_date'):
            birth = datetime.strptime(user['birth_date'], '%Y-%m-%d')
            age = (datetime.now() - birth).days / 365
        else:
            age = 'Unknown'
        
        return {
            'name': name,
            'email': email,
            'age': age
        }

# Usage
manager = UserManager('https://api.example.com/v1', 'secret-key-123')
user = manager.get_user(42)
if user:
    print manager.format_user(user)

users = manager.get_users({'status': 'active', 'role': 'admin'})
for u in users:
    print u['email']
```

## Current State
- **Original Language Version**: Python 2.7
- **Target Language Version**: Python 3.11+
- **Age**: Written in 2015 (~9 years old)
- **Last Modified**: 2018 (minor bug fix)

## Modernization Goals
Update to use:
- [x] Modern language features:
  - f-strings instead of % formatting
  - Type hints
  - Dataclasses or Pydantic models
  - dict comprehensions
  - requests library instead of urllib2
- [x] Current standard library APIs (urllib.request if no requests)
- [x] Type hints/annotations for all functions
- [x] Modern error handling (specific exceptions, logging)
- [x] Async/await for API calls (make it async-capable)
- [x] Current security best practices (don't print sensitive data, use secrets for API keys)
- [x] Modern testing approaches (show structure)
- [x] Updated dependencies (requests, aiohttp for async)
- [x] Current coding conventions (PEP 8, docstrings)

## Known Issues
- Uses deprecated urllib2 (removed in Python 3)
- Uses .has_key() method (removed in Python 3)
- Uses .iteritems() (removed in Python 3, use .items())
- Python 2 exception syntax (except Exception, e:)
- No type hints
- Poor error handling (prints instead of logging)
- No async support
- No retries or timeout handling
- Hardcoded date format
- Age calculation is naive (doesn't account for leap years properly)
- API key in plaintext

## Constraints
- **Backward Compatibility**: Can break - this is a major upgrade
- **Testing**: No existing tests, need to create comprehensive test suite
- **Deployment**: Full replacement (no Python 2 support needed)
- **Dependencies**: Can update freely, prefer modern libraries

## Context
- **Production Usage**: Used in several internal tools, not customer-facing
- **Change Frequency**: Rarely modified, but used daily
- **Technical Debt**: Known to be brittle, failures happen weekly

## Output Format
Provide:
1. Modernized code with type hints, docstrings, and explanatory comments
2. List of all changes made with reasoning
3. Migration guide for existing code using this class
4. Example unit tests using pytest
5. requirements.txt with updated dependencies
6. Environment variable handling for API keys
7. Configuration class for settings
```

## Tips

- **Incremental**: Break large modernizations into smaller steps
- **Tests**: Write tests before modernizing to prevent regressions
- **Features**: Use new language features appropriately, not just for novelty
- **Dependencies**: Update dependencies carefully, check for breaking changes
- **Documentation**: Document what changed and why for the team
