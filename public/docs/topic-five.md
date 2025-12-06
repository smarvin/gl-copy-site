---
title: Bulk API Support
layout: default
permalink: /docs/topic-five/
---

# Bulk API Support

The Bulk API allows creating or updating tasks in batches.

## Example:
```
POST /tasks/bulk
```

### Request Body:
```json
{
  "tasks": [
    { "title": "Task 1", "priority": "high" },
    { "title": "Task 2", "priority": "low" }
    { "title": "Task 3", "priority": "medium" }
  ]
}
```

### Response:
```json
{
  "created": ["task_101", "task_102"],
  "errors": []
}
```
