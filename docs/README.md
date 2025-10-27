# Documentation (docs/)

## Overview
Comprehensive project documentation including API references, user guides, and developer guides.

## Documentation Structure

### api_reference.md
Complete API documentation:
- Endpoint descriptions
- Request/response formats
- Authentication
- Rate limits
- Examples

### user_guide.md
Guide for end users:
- Getting started
- Features overview
- Common workflows
- Troubleshooting
- FAQs

### developer_guide.md
Guide for developers:
- Architecture overview
- Module documentation
- Development setup
- Contributing guidelines
- Code standards

### data_dictionary.md
Data model documentation:
- Database schema
- Field descriptions
- Relationships
- Constraints
- Examples

## Creating Documentation

### Markdown Format
Use markdown for all documentation:
- Clear headings
- Code examples
- Links to related docs
- Tables for reference data

### Example Structure
```markdown
# Feature Name

## Overview
Brief description of the feature.

## Usage
How to use the feature.

## Examples
Code examples.

## API Reference
Detailed API information.

## See Also
Links to related documentation.
```

## Updating Documentation

### When to Update
- New features added
- API changes
- Bug fixes affecting behavior
- Configuration changes

### Documentation Review
- Keep docs in sync with code
- Review during pull requests
- Test code examples
- Update version numbers

## Documentation Tools

### Generate API Docs
```bash
# Using Sphinx
sphinx-apidoc -o docs/api src/
sphinx-build -b html docs docs/_build

# Using pdoc
pdoc --html --output-dir docs src/
```

### Serve Documentation Locally
```bash
# Using MkDocs
mkdocs serve

# Or simple HTTP server
python -m http.server 8000 -d docs/
```

## Related Files
- [README.md](../README.md) - Project overview
- [DESIGN.md](../DESIGN.md) - Design document
- [PROJECT_STRUCTURE.md](../PROJECT_STRUCTURE.md) - Structure guide
