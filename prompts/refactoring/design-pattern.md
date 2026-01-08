# Design Pattern Application Prompt

Use this template to apply appropriate design patterns to your code.

## Prompt Template

```
Please refactor the following code to apply the [PATTERN NAME] design pattern.

## Current Code
```[language]
[Paste code here]
```

## Pattern Requirements
**Pattern to Apply**: [Strategy, Factory, Observer, Singleton, etc.]
**Why This Pattern**: [Reason for choosing this pattern]
**Expected Benefits**: [What will improve]

## Current Problems
1. [Problem 1 that pattern will solve]
2. [Problem 2 that pattern will solve]

## Context
- **Language**: [Programming language and version]
- **Framework**: [If applicable]
- **Existing Architecture**: [Brief description]

## Requirements
- Implement pattern correctly following best practices
- Maintain existing functionality
- Improve extensibility for future changes
- Include usage examples
- Add comments explaining the pattern

## Alternative Patterns (Optional)
If [PATTERN NAME] is not ideal, suggest and implement a better alternative.

## Output Format
Provide:
1. Refactored code with pattern applied
2. Explanation of how pattern is implemented
3. Class/sequence diagram (ASCII or description)
4. Usage examples
5. Benefits gained from this pattern
6. Potential drawbacks or trade-offs
```

## Example Usage

```
Please refactor the following code to apply an appropriate design pattern (suggest the best one).

## Current Code
```python
class ReportGenerator:
    def __init__(self, data):
        self.data = data
    
    def generate(self, format_type):
        if format_type == 'pdf':
            # PDF generation logic
            pdf_content = f"PDF Report\n{'='*50}\n"
            for item in self.data:
                pdf_content += f"{item['name']}: {item['value']}\n"
            return pdf_content
        
        elif format_type == 'html':
            # HTML generation logic
            html_content = "<html><body><h1>Report</h1><table>"
            for item in self.data:
                html_content += f"<tr><td>{item['name']}</td><td>{item['value']}</td></tr>"
            html_content += "</table></body></html>"
            return html_content
        
        elif format_type == 'csv':
            # CSV generation logic
            csv_content = "Name,Value\n"
            for item in self.data:
                csv_content += f"{item['name']},{item['value']}\n"
            return csv_content
        
        elif format_type == 'json':
            # JSON generation logic
            import json
            return json.dumps(self.data, indent=2)
        
        elif format_type == 'xml':
            # XML generation logic
            xml_content = "<?xml version='1.0'?>\n<report>\n"
            for item in self.data:
                xml_content += f"  <item>\n    <name>{item['name']}</name>\n    <value>{item['value']}</value>\n  </item>\n"
            xml_content += "</report>"
            return xml_content
        
        else:
            raise ValueError(f"Unsupported format: {format_type}")

# Usage
data = [
    {'name': 'Revenue', 'value': 100000},
    {'name': 'Expenses', 'value': 75000},
    {'name': 'Profit', 'value': 25000}
]

generator = ReportGenerator(data)
pdf_report = generator.generate('pdf')
html_report = generator.generate('html')
csv_report = generator.generate('csv')
```

## Pattern Requirements
**Pattern to Apply**: Suggest the most appropriate pattern (I'm thinking Strategy or Factory)
**Why This Pattern**: 
- Need to add more formats easily (Excel, Markdown, LaTeX)
- Each format has complex generation logic
- Want to test each format independently
**Expected Benefits**: 
- Easier to add new formats
- Better separation of concerns
- More testable code

## Current Problems
1. Violates Open/Closed Principle (must modify class to add formats)
2. God class - too many responsibilities
3. Growing if-elif chain is hard to maintain
4. Difficult to unit test individual format generators
5. Format-specific logic mixed with orchestration logic

## Context
- **Language**: Python 3.11
- **Framework**: None (standalone utility)
- **Existing Architecture**: Simple utility class used in web app and CLI tool

## Requirements
- Implement pattern correctly following Python best practices
- Maintain existing functionality (same output for same inputs)
- Make it easy to add new formats (plugin-style)
- Improve extensibility for format-specific options (e.g., PDF with images, CSV with custom delimiter)
- Include usage examples showing how to add a new format
- Add type hints and docstrings

## Alternative Patterns
If Strategy or Factory is not ideal, suggest and implement a better alternative. Consider:
- Strategy Pattern: Different algorithms for same task
- Factory Pattern: Creating format generators dynamically
- Command Pattern: Encapsulating requests
- Template Method: Common algorithm with format-specific steps
- Or a combination

## Output Format
Provide:
1. Refactored code with pattern applied
2. Explanation of how pattern is implemented and why it's the best choice
3. Class diagram (ASCII art or description of relationships)
4. Usage examples including:
   - Using existing formats
   - Adding a new format (show how to extend)
   - Configuring format-specific options
5. Benefits gained from this pattern
6. Potential drawbacks or trade-offs
7. Unit test structure recommendation
```

## Tips

- **Pattern Purpose**: Choose patterns based on problem, not popularity
- **YAGNI**: Don't over-engineer - apply patterns when they solve real problems
- **Language Idioms**: Adapt patterns to language features (e.g., Python's duck typing)
- **Documentation**: Explain the pattern for future maintainers
- **Trade-offs**: Be aware of added complexity vs. benefits gained
