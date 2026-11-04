# Land Kit AI Reference Documentation

This directory contains structured JSON files designed for AI assistants (like Raven in Grasshopper) to understand and generate Land Kit scripts.

## Files Overview

### 1. **component_registry.json**
Complete catalog of all 123 Land Kit components with:
- Component name, nickname, category
- Input/output specifications with types
- Brief descriptions
- File references

**Use case:** AI needs to know what components exist and how to call them.

**Access URL:** `https://raw.githubusercontent.com/landau-design/land-kit/main/docs/ai-reference/component_registry.json`

```json
{
  "name": "Add Shape Values Layer",
  "nickname": "+ShapeValues",
  "inputs": [
    {"name": "<E>", "type": "generic", "required": true},
    {"name": "name", "type": "string", "required": false}
  ]
}
```

---

### 2. **layer_validation.json**
Complete reference for all environment layers including:
- Layer names and descriptions
- Valid values for categorical layers (HSG, GC, Condition)
- Data types and acceptable ranges
- Dependencies between layers
- Which components create/require each layer

**Use case:** AI needs to validate data and understand what values are acceptable.

**Access URL:** `https://raw.githubusercontent.com/landau-design/land-kit/main/docs/ai-reference/layer_validation.json`

**Example queries:**
- "What are valid values for HSG?" → `["A", "B", "C", "D"]`
- "What does GC mean?" → Ground Cover type
- "What layers does Calculate CN need?" → `["hsg", "gc", "condition"]`

---

### 3. **workflow_patterns.json**
Pre-defined workflow sequences for common tasks:
- Basic Hydrology Analysis
- CN Calculation
- Solar Analysis
- Drainage Design
- Custom Analysis

Each workflow includes:
- Step-by-step component order
- Component inputs/outputs for each step
- What layers are created
- Use cases and prerequisites

**Use case:** AI can follow a proven sequence instead of building from scratch.

**Access URL:** `https://raw.githubusercontent.com/landau-design/land-kit/main/docs/ai-reference/workflow_patterns.json`

**Dependency graph** shows what must run before other components.

---

### 4. **error_reference.json**
Error messages, causes, and solutions:
- 15+ documented error scenarios
- Causes and symptoms
- Step-by-step solutions
- Common troubleshooting patterns
- Performance guidelines

**Use case:** When something fails, AI can help debug with known solutions.

**Access URL:** `https://raw.githubusercontent.com/landau-design/land-kit/main/docs/ai-reference/error_reference.json`

---

## For AI/Tool Developers

### Recommended Load Order
1. Load `component_registry.json` → know what components exist
2. Load `layer_validation.json` → understand data requirements
3. Load `workflow_patterns.json` → learn proven sequences
4. Load `error_reference.json` → prepare error handling

### File Format
- **Type:** JSON (machine-readable)
- **Version field:** Check `meta.version` to detect outdated data
- **Encoding:** UTF-8

### Implementation Example (Python)

```python
import urllib2
import json

# Fetch component registry
url = "https://raw.githubusercontent.com/landau-design/land-kit/main/docs/ai-reference/component_registry.json"
response = urllib2.urlopen(url)
registry = json.load(response)

# Find a component by nickname
for comp in registry['components']:
    if comp['nickname'] == '+ShapeValues':
        print(f"Component: {comp['name']}")
        print(f"Inputs: {[i['name'] for i in comp['inputs']]}")
        break

# Check layer validation rules
validation_url = "https://raw.githubusercontent.com/landau-design/land-kit/main/docs/ai-reference/layer_validation.json"
val_response = urllib2.urlopen(validation_url)
validation = json.load(val_response)

# Validate HSG value
valid_hsg = validation['validation_rules']['HSG_values']['allowed']  # ["A", "B", "C", "D"]
```

---

## Updating These Files

These files are auto-generated/maintained in the Land Kit repository. To update:

1. Component changes → `component_registry.json` regenerated from source
2. New layer types → Update `layer_validation.json` manually
3. New workflows → Add to `workflow_patterns.json` manually
4. New errors → Add to `error_reference.json` manually

Check the repository `README.md` for contribution guidelines.

---

## Version History

- **2.3.1** (2026-11-04) - Initial AI reference documentation
  - 123 components documented
  - 18 layers with validation rules
  - 5 workflow patterns
  - 15+ error scenarios

---

## Support

For questions about these reference files or Land Kit integration with AI tools:
- Check the main Land Kit repository
- Review component documentation in Grasshopper
- Refer to workflow examples in `workflow_patterns.json`
