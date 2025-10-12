# Story_For_GameBook

Repository containing interactive stories used by the [GameBook project](https://github.com/corentinffoucault/GameBook).

Each story is stored as a JSON file following a defined schema, ready to be loaded and played in the GameBook engine.

## JSON Structure

Below is the general structure and meaning of each field.

```jsonc
{
  "author": "string",              // Author of the story
  "firstNode": "string",           // ID of the first node
  "nodes": [                       // List of all story nodes
    {
      "id": "string",              // Unique node identifier
      "title": "string",           // Node title
      "text": "string",            // Main narrative text

      "choice": [                  // Available choices from this node
        {
          "type": "direct | conditional", // Choice type
          "name": "string",               // Short label for the choice
          "text": "string",               // Description or action text
          "next": "string",               // Next node ID if the choice is successful
          
          // Optional fields for conditional choices:
          "nextFail": "string",           // Node ID if the condition fails
          "condition": {                  // Condition to evaluate
            "type": "string",             // e.g., "attribute" or "Gold"
            "attribute": "string",        // Name of the attribute to check (optional)
            "value": "number",            // Value for comparison (optional)
            "comparator": "< | <= | > | >= | == | !=" // Comparison operator
          },
          "success": "string",            // Message displayed when condition passes
          "fail": "string"                // Message displayed when condition fails
        }
      ]
    }
  ]
}
```

---

## 🧱 Field Reference

| Field       | Type          | Required   | Description                       |
|-------------|---------------|------------|-----------------------------------|
| `author`    | `string`      | ✅         | Name of the story’s author        |
| `firstNode` | `string`      | ✅         | ID of the first node in the story |
| `nodes`     | `array<Node>` | ✅         | All story nodes                   |

### Node Object (`Node`)
| Field    | Type            | Required   | Description                       |
|----------|-----------------|------------|-----------------------------------|
| `id`     | `string`        | ✅         | Unique node identifier            |
| `title`  | `string`        | ✅         | Title or short label for the node |
| `text`   | `string`        | ✅         | Narrative content                 |
| `choice` | `array<Choice>` | ✅         | Choices available from this node  |

### Choice Object (`Choice`)
| Field       | Type     | Required   | Description                                |
|-------------|----------|------------|--------------------------------------------|
| `type`      | `string` | ✅         | `"direct"` or `"conditional"`              |
| `name`      | `string` | ✅         | Short label for the choice                 |
| `text`      | `string` | ✅         | Narrative text for this choice             |
| `next`      | `string` | ✅         | ID of the next node if successful          |
| `nextFail`  | `string` | ❌         | ID of the next node if the condition fails |
| `condition` | `object` | ❌         | Condition to check (see below)             |
| `success`   | `string` | ❌         | Message if the condition succeeds          |
| `fail`      | `string` | ❌         | Message if the condition fails             |

### Condition Object (`Condition`)
| Field        | Type     | Required  | Description                                   |
|--------------|----------|-----------|-----------------------------------------------|
| `type`       | `string` | ✅        | Type of condition (e.g., `attribute`, `Gold`) |
| `attribute`  | `string` | ❌        | Attribute name to check                       |
| `value`      | `number` | ❌        | Numeric value to compare against              |
| `comparator` | `string` | ✅        | Comparison operator (`<`, `>`, `==`, etc.)    |

---

## Example

```json
{
  "author": "Author",
  "firstNode": "Node 1",
  "nodes": [
    {
      "id": "Node 1",
      "title": "Arrival",
      "text": "The hero arrives from the south. It's cold, snow covers the town.",
      "choice": [
        {
          "type": "direct",
          "name": "Go to the tavern",
          "text": "You head north toward the tavern to warm up.",
          "next": "Node 1.1"
        },
        {
          "type": "conditional",
          "name": "Jump over the fence",
          "text": "You try to jump over the fence.",
          "next": "Node 2",
          "nextFail": "Node 3",
          "condition": {
            "type": "attribute",
            "attribute": "AG",
            "comparator": ">"
          },
          "success": "You jumped successfully!",
          "fail": "You failed to jump."
        }
      ]
    }
  ]
}
```
