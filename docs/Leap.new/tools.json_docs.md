# Documentation: tools.json

## File Metadata

- **Path**: `Leap.new/tools.json`
- **Size**: 16553 bytes (16.17 KB)
- **MIME Type**: application/json
- **Lines**: 516
- **Words**: 1312
- **Last Modified**: 2025-11-15T09:41:05.386333

## Original Source

```json
{
  "tools": [
    {
      "name": "create_artifact",
      "description": "Creates a comprehensive artifact containing all project files for building full-stack applications with Encore.ts backend and React frontend",
      "parameters": {
        "type": "object",
        "properties": {
          "id": {
            "type": "string",
            "description": "Descriptive identifier for the project in snake-case (e.g., 'todo-app', 'blog-platform')"
          },
          "title": {
            "type": "string",
            "description": "Human-readable title for the project (e.g., 'Todo App', 'Blog Platform')"
          },
          "commit": {
            "type": "string",
            "description": "Brief description of changes in 3-10 words max"
          },
          "files": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "path": {
                  "type": "string",
                  "description": "Relative file path from project root"
                },
                "content": {
                  "type": "string",
                  "description": "Complete file content - NEVER use placeholders or truncation"
                },
                "action": {
                  "type": "string",
                  "enum": ["create", "modify", "delete", "move"],
                  "description": "Action to perform on the file"
                },
                "from": {
                  "type": "string",
                  "description": "Source path for move operations"
                },
                "to": {
                  "type": "string", 
                  "description": "Target path for move operations"
                }
              },
              "required": ["path", "action"]
            }
          }
        },
        "required": ["id", "title", "commit", "files"]
      }
    },
    {
      "name": "define_backend_service",
      "description": "Defines an Encore.ts backend service with proper structure",
      "parameters": {
        "type": "object",
        "properties": {
          "serviceName": {
            "type": "string",
            "description": "Name of the backend service"
          },
          "endpoints": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "name": {
                  "type": "string",
                  "description": "Unique endpoint name"
                },
                "method": {
                  "type": "string",
                  "enum": ["GET", "POST", "PUT", "DELETE", "PATCH"],
                  "description": "HTTP method"
                },
                "path": {
                  "type": "string",
                  "description": "API path with parameters (e.g., '/users/:id')"
                },
                "expose": {
                  "type": "boolean",
                  "description": "Whether endpoint is publicly accessible"
                },
                "auth": {
                  "type": "boolean",
                  "description": "Whether endpoint requires authentication"
                }
              },
              "required": ["name", "method", "path"]
            }
          },
          "database": {
            "type": "object",
            "properties": {
              "name": {
                "type": "string",
                "description": "Database name"
              },
              "tables": {
                "type": "array",
                "items": {
                  "type": "object",
                  "properties": {
                    "name": {
                      "type": "string",
                      "description": "Table name"
                    },
                    "columns": {
                      "type": "array",
                      "items": {
                        "type": "object",
                        "properties": {
                          "name": {
                            "type": "string"
                          },
                          "type": {
                            "type": "string"
                          },
                          "constraints": {
                            "type": "string"
                          }
                        },
                        "required": ["name", "type"]
                      }
                    }
                  },
                  "required": ["name", "columns"]
                }
              }
            }
          }
        },
        "required": ["serviceName"]
      }
    },
    {
      "name": "create_react_component",
      "description": "Creates a React component with TypeScript and Tailwind CSS",
      "parameters": {
        "type": "object",
        "properties": {
          "componentName": {
            "type": "string",
            "description": "Name of the React component"
          },
          "path": {
            "type": "string",
            "description": "Path where component should be created"
          },
          "props": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "name": {
                  "type": "string"
                },
                "type": {
                  "type": "string"
                },
                "optional": {
                  "type": "boolean"
                }
              },
              "required": ["name", "type"]
            }
          },
          "useBackend": {
            "type": "boolean",
            "description": "Whether component uses backend API calls"
          },
          "styling": {
            "type": "object",
            "properties": {
              "theme": {
                "type": "string",
                "enum": ["light", "dark", "system"],
                "description": "Component theme"
              },
              "responsive": {
                "type": "boolean",
                "description": "Whether component is responsive"
              },
              "animations": {
                "type": "boolean",
                "description": "Whether to include subtle animations"
              }
            }
          }
        },
        "required": ["componentName", "path"]
      }
    },
    {
      "name": "setup_authentication",
      "description": "Sets up authentication using Clerk for both backend and frontend",
      "parameters": {
        "type": "object",
        "properties": {
          "provider": {
            "type": "string",
            "enum": ["clerk"],
            "description": "Authentication provider"
          },
          "features": {
            "type": "array",
            "items": {
              "type": "string",
              "enum": ["sign-in", "sign-up", "user-profile", "session-management"]
            }
          },
          "protectedRoutes": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "API endpoints that require authentication"
          }
        },
        "required": ["provider"]
      }
    },
    {
      "name": "create_database_migration",
      "description": "Creates a new SQL migration file for Encore.ts database",
      "parameters": {
        "type": "object",
        "properties": {
          "migrationName": {
            "type": "string",
            "description": "Descriptive name for the migration"
          },
          "version": {
            "type": "integer",
            "description": "Migration version number"
          },
          "operations": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "type": {
                  "type": "string",
                  "enum": ["CREATE_TABLE", "ALTER_TABLE", "DROP_TABLE", "CREATE_INDEX", "DROP_INDEX"]
                },
                "sql": {
                  "type": "string",
                  "description": "Raw SQL for the operation"
                }
              },
              "required": ["type", "sql"]
            }
          }
        },
        "required": ["migrationName", "version", "operations"]
      }
    },
    {
      "name": "setup_streaming_api",
      "description": "Sets up streaming APIs for real-time communication",
      "parameters": {
        "type": "object",
        "properties": {
          "streamType": {
            "type": "string",
            "enum": ["streamIn", "streamOut", "streamInOut"],
            "description": "Type of streaming API"
          },
          "endpoint": {
            "type": "string",
            "description": "Stream endpoint path"
          },
          "messageTypes": {
            "type": "object",
            "properties": {
              "handshake": {
                "type": "object",
                "description": "Handshake message schema"
              },
              "incoming": {
                "type": "object",
                "description": "Incoming message schema"
              },
              "outgoing": {
                "type": "object",
                "description": "Outgoing message schema"
              }
            }
          }
        },
        "required": ["streamType", "endpoint"]
      }
    },
    {
      "name": "configure_secrets",
      "description": "Configures secret management for API keys and sensitive data",
      "parameters": {
        "type": "object",
        "properties": {
          "secrets": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "name": {
                  "type": "string",
                  "description": "Secret name (e.g., 'OpenAIKey', 'DatabaseURL')"
                },
                "description": {
                  "type": "string",
                  "description": "Description of what the secret is used for"
                },
                "required": {
                  "type": "boolean",
                  "description": "Whether this secret is required for the app to function"
                }
              },
              "required": ["name", "description"]
            }
          }
        },
        "required": ["secrets"]
      }
    },
    {
      "name": "setup_object_storage",
      "description": "Sets up object storage buckets for file uploads",
      "parameters": {
        "type": "object",
        "properties": {
          "buckets": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "name": {
                  "type": "string",
                  "description": "Bucket name"
                },
                "public": {
                  "type": "boolean",
                  "description": "Whether bucket contents are publicly accessible"
                },
                "versioned": {
                  "type": "boolean",
                  "description": "Whether to enable object versioning"
                },
                "allowedFileTypes": {
                  "type": "array",
                  "items": {
                    "type": "string"
                  },
                  "description": "Allowed file MIME types"
                }
              },
              "required": ["name"]
            }
          }
        },
        "required": ["buckets"]
      }
    },
    {
      "name": "setup_pubsub",
      "description": "Sets up Pub/Sub topics and subscriptions for event-driven architecture",
      "parameters": {
        "type": "object",
        "properties": {
          "topics": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "name": {
                  "type": "string",
                  "description": "Topic name"
                },
                "eventSchema": {
                  "type": "object",
                  "description": "TypeScript interface for event data"
                },
                "deliveryGuarantee": {
                  "type": "string",
                  "enum": ["at-least-once", "exactly-once"],
                  "description": "Message delivery guarantee"
                }
              },
              "required": ["name", "eventSchema"]
            }
          },
          "subscriptions": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "name": {
                  "type": "string",
                  "description": "Subscription name"
                },
                "topicName": {
                  "type": "string",
                  "description": "Name of topic to subscribe to"
                },
                "handler": {
                  "type": "string",
                  "description": "Handler function description"
                }
              },
              "required": ["name", "topicName", "handler"]
            }
          }
        },
        "required": ["topics"]
      }
    },
    {
      "name": "create_test_suite",
      "description": "Creates test suites using Vitest for backend and frontend",
      "parameters": {
        "type": "object",
        "properties": {
          "testType": {
            "type": "string",
            "enum": ["backend", "frontend", "integration"],
            "description": "Type of tests to create"
          },
          "testFiles": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "path": {
                  "type": "string",
                  "description": "Test file path"
                },
                "description": {
                  "type": "string",
                  "description": "What the test file covers"
                },
                "testCases": {
                  "type": "array",
                  "items": {
                    "type": "object",
                    "properties": {
                      "name": {
                        "type": "string"
                      },
                      "description": {
                        "type": "string"
                      }
                    },
                    "required": ["name"]
                  }
                }
              },
              "required": ["path", "testCases"]
            }
          }
        },
        "required": ["testType", "testFiles"]
      }
    }
  ],
  "guidelines": {
    "code_quality": [
      "Use 2 spaces for indentation",
      "Split functionality into smaller, focused modules",
      "Keep files as small as possible",
      "Use proper TypeScript typing throughout",
      "Follow consistent naming conventions",
      "Include comprehensive error handling",
      "Add meaningful comments for complex logic"
    ],
    "backend_requirements": [
      "All backend code must use Encore.ts",
      "Store data using SQL Database or Object Storage",
      "Never store data in memory or local files",
      "All services go under backend/ folder",
      "Each API endpoint in its own file",
      "Unique endpoint names across the application",
      "Use template literals for database queries",
      "Document all API endpoints with comments"
    ],
    "frontend_requirements": [
      "Use React with TypeScript and Tailwind CSS",
      "Import backend client as: import backend from '~backend/client'",
      "Use shadcn/ui components when appropriate",
      "Create responsive designs for all screen sizes",
      "Include subtle animations and interactions",
      "Use proper error handling with console.error logs",
      "Split components into smaller, reusable modules",
      "Frontend code goes in frontend/ folder (no src/ subfolder)"
    ],
    "file_handling": [
      "Always provide FULL file content",
      "NEVER use placeholders or truncation",
      "Only output files that need changes",
      "Use leapFile for creates/modifications",
      "Use leapDeleteFile for deletions",
      "Use leapMoveFile for renames/moves",
      "Exclude auto-generated files (package.json, etc.)"
    ],
    "security": [
      "Use secrets for all sensitive data",
      "Implement proper authentication when requested",
      "Validate all user inputs",
      "Use proper CORS settings",
      "Follow security best practices for APIs"
    ]
  }
}


```

## High-Level Overview

This file defines tool configurations and schemas. It appears to be a tools definition file for an AI agent or code assistant.

### Purpose
Configuration file defining available tools, functions, and their schemas for agent interaction.

### Key Components
- Tool definitions with names, descriptions, and parameters
- Schema validation rules
- Function signatures and documentation


## Detailed Walkthrough

### File Structure

This file contains 516 lines of json content.

### JSON Structure

This JSON file contains a dictionary with 2 top-level keys:

- `tools`: list
- `guidelines`: dict


## Usage and Examples

### How to Use

This JSON file can be loaded and used programmatically:

```python
import json

with open('Leap.new/tools.json', 'r') as f:
    data = json.load(f)
```


## Performance and Security Notes

### Performance Considerations
- File size: 16.17 KB
- Load time: negligible for files under 1MB
- JSON parsing overhead minimal for this file size


### Security Considerations
⚠️ **WARNING**: This file may contain sensitive information. Found keywords: secret

Please ensure:
- Sensitive data is properly redacted in production
- This file is not committed to public repositories
- Access controls are properly configured


## Related Files

- [`Prompts.txt`](./Prompts.txt_docs.md)


## Testing and Validation

### How to Test
Validate JSON syntax:

```bash
python -c "import json; json.load(open('Leap.new/tools.json'))"
# or
jq . 'Leap.new/tools.json'
```


---

*Generated by Repo Book Generator v1.0.0 on 2025-11-15 09:46:00*
