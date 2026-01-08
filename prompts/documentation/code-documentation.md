# Code Documentation Prompt

Use this template to generate inline code documentation and docstrings.

## Prompt Template

```
Please add comprehensive documentation to the following code.

## Code to Document
```[language]
[Paste your code here]
```

## Documentation Style
- **Language**: [Python, JavaScript, Java, etc.]
- **Format**: [JSDoc, Google Style, NumPy Style, JavaDoc, etc.]
- **Detail Level**: [Minimal, Standard, Verbose]

## Documentation Requirements
Please include:
- [ ] Module/file-level documentation
- [ ] Class documentation
- [ ] Method/function documentation
- [ ] Parameter descriptions (types and purpose)
- [ ] Return value descriptions
- [ ] Exception/error documentation
- [ ] Usage examples
- [ ] Notes on complexity or performance
- [ ] Links to related functions/classes
- [ ] Deprecation warnings (if applicable)

## Special Focus
[Any particular aspects to emphasize in documentation]

## Target Audience
- **Intended Readers**: [New developers, API users, maintainers, etc.]
- **Assumed Knowledge**: [What background knowledge to assume]

## Output Format
Return the code with:
1. All documentation added inline
2. Formatted according to specified style guide
3. Clear and concise descriptions
4. Practical examples where helpful
```

## Example Usage

```
Please add comprehensive documentation to the following code.

## Code to Document
```python
class UserRepository:
    def __init__(self, db_session):
        self.db = db_session
    
    def create(self, user_data):
        user = User(**user_data)
        self.db.add(user)
        self.db.commit()
        return user
    
    def get_by_id(self, user_id):
        return self.db.query(User).filter(User.id == user_id).first()
    
    def get_by_email(self, email):
        return self.db.query(User).filter(User.email == email).first()
    
    def update(self, user_id, update_data):
        user = self.get_by_id(user_id)
        if not user:
            return None
        for key, value in update_data.items():
            setattr(user, key, value)
        self.db.commit()
        return user
    
    def delete(self, user_id):
        user = self.get_by_id(user_id)
        if user:
            self.db.delete(user)
            self.db.commit()
            return True
        return False
    
    def list_all(self, skip=0, limit=100, filters=None):
        query = self.db.query(User)
        if filters:
            for key, value in filters.items():
                query = query.filter(getattr(User, key) == value)
        return query.offset(skip).limit(limit).all()
```

## Documentation Style
- **Language**: Python
- **Format**: Google Style Python docstrings
- **Detail Level**: Standard (comprehensive but not overly verbose)

## Documentation Requirements
Please include:
- [x] Module/file-level documentation (add at top)
- [x] Class documentation
- [x] Method/function documentation
- [x] Parameter descriptions (types and purpose)
- [x] Return value descriptions
- [x] Exception/error documentation (if methods can raise exceptions)
- [x] Usage examples (for class and complex methods)
- [ ] Notes on complexity or performance (if relevant)
- [x] Links to related functions/classes (if applicable)
- [ ] Deprecation warnings (not applicable here)

## Special Focus
- Emphasize the repository pattern being used
- Explain database session handling
- Document the filtering mechanism in list_all
- Note any potential None returns

## Target Audience
- **Intended Readers**: New developers joining the project
- **Assumed Knowledge**: Basic Python, SQLAlchemy ORM, repository pattern

## Output Format
Return the code with:
1. All documentation added inline
2. Formatted according to Google Style docstring format
3. Clear descriptions with type hints in docstrings
4. Practical example in class docstring
5. Add module-level docstring at the top explaining this is the user repository
```

## Tips

- **Consistency**: Follow the project's existing documentation style
- **Clarity**: Write for someone unfamiliar with the code
- **Examples**: Include usage examples for complex APIs
- **Types**: Document parameter and return types clearly
- **Maintenance**: Write docs that will stay relevant as code evolves
