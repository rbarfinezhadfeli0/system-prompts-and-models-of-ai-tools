# Documentation: Builder Tools.json

## File Metadata

- **Path**: `Trae/Builder Tools.json`
- **Size**: 8264 bytes (8.07 KB)
- **MIME Type**: application/json
- **Lines**: 222
- **Words**: 903
- **Last Modified**: 2025-11-15T09:41:05.420333

## Original Source

```json
{
  "todo_write": {
    "description": "Use this tool to create and manage a structured task list for your current coding session. This helps you track progress, organize complex tasks, and demonstrate thoroughness to the user. It also helps the user understand the progress of the task and overall progress of their requests.",
    "params": {
      "type": "object",
      "properties": {
        "todos": {
          "description": "The updated todo list",
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "content": {"type": "string"},
              "status": {"type": "string", "enum": ["pending", "in_progress", "completed"]},
              "id": {"type": "string"},
              "priority": {"type": "string", "enum": ["high", "medium", "low"]}
            },
            "required": ["content", "status", "id", "priority"],
            "minItems": 3,
            "maxItems": 10
          }
        }
      },
      "required": ["todos"]
    }
  },
  "search_codebase": {
    "description": "This tool is Trae's context engine. It: 1. Takes in a natural language description of the code you are looking for; 2. Uses a proprietary retrieval/embedding model suite that produces the highest-quality recall of relevant code snippets from across the codebase; 3. Maintains a real-time index of the codebase, so the results are always up-to-date and reflects the current state of the codebase; 4. Can retrieve across different programming languages; 5. Only reflects the current state of the codebase on the disk, and has no information on version control or code history.",
    "params": {
      "type": "object",
      "properties": {
        "information_request": {"type": "string"},
        "target_directories": {"type": "array", "items": {"type": "string"}}
      },
      "required": ["information_request"]
    }
  },
  "search_by_regex": {
    "description": "Fast text-based search that finds exact pattern matches within files or directories, utilizing the ripgrep command for efficient searching.",
    "params": {
      "type": "object",
      "properties": {
        "query": {"type": "string"},
        "search_directory": {"type": "string"}
      },
      "required": ["query"]
    }
  },
  "view_files": {
    "description": "View up to 3 files simultaneously in batch mode for faster information gathering.",
    "params": {
      "type": "object",
      "properties": {
        "files": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "file_path": {"type": "string"},
              "start_line_one_indexed": {"type": "integer"},
              "end_line_one_indexed_inclusive": {"type": "integer"},
              "read_entire_file": {"type": "boolean"}
            },
            "required": ["file_path", "start_line_one_indexed", "end_line_one_indexed_inclusive"]
          }
        }
      },
      "required": ["files"]
    }
  },
  "list_dir": {
    "description": "You can use this tool to view files of the specified directory.",
    "params": {
      "type": "object",
      "properties": {
        "dir_path": {"type": "string"},
        "max_depth": {"type": "integer", "default": 3}
      },
      "required": ["dir_path"]
    }
  },
  "write_to_file": {
    "description": "You can use this tool to write content to a file with precise control over creation/rewrite behavior.",
    "params": {
      "type": "object",
      "properties": {
        "rewrite": {"type": "boolean"},
        "file_path": {"type": "string"},
        "content": {"type": "string"}
      },
      "required": ["rewrite", "file_path", "content"]
    }
  },
  "update_file": {
    "description": "You can use this tool to edit file, if you think that using this tool is more cost-effective than other available editing tools, you should choose this tool, otherwise you should choose other available edit tools.",
    "params": {
      "type": "object",
      "properties": {
        "file_path": {"type": "string"},
        "replace_blocks": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "old_str": {"type": "string"},
              "new_str": {"type": "string"}
            },
            "required": ["old_str", "new_str"]
          }
        }
      },
      "required": ["file_path", "replace_blocks"]
    }
  },
  "edit_file_fast_apply": {
    "description": "You can use this tool to edit an existing files with less than 1000 lines of code, and you should follow these rules:",
    "params": {
      "type": "object",
      "properties": {
        "file_path": {"type": "string"},
        "content": {"type": "string"},
        "instruction": {"type": "string", "default": ""},
        "code_language": {"type": "string"}
      },
      "required": ["file_path", "content"]
    }
  },
  "rename_file": {
    "description": "You can use this tool to move or rename an existing file.",
    "params": {
      "type": "object",
      "properties": {
        "file_path": {"type": "string"},
        "rename_file_path": {"type": "string"}
      },
      "required": ["file_path", "rename_file_path"]
    }
  },
  "delete_file": {
    "description": "You can use this tool to delete files, you can delete multi files in one toolcall, and you MUST make sure the files is exist before deleting.",
    "params": {
      "type": "object",
      "properties": {
        "file_paths": {"type": "array", "items": {"type": "string"}}
      },
      "required": ["file_paths"]
    }
  },
  "run_command": {
    "description": "You can use this tool to PROPOSE a command to run on behalf of the user.",
    "params": {
      "type": "object",
      "properties": {
        "command": {"type": "string"},
        "target_terminal": {"type": "string"},
        "command_type": {"type": "string"},
        "cwd": {"type": "string"},
        "blocking": {"type": "boolean"},
        "wait_ms_before_async": {"type": "integer", "minimum": 0},
        "requires_approval": {"type": "boolean"}
      },
      "required": ["command", "blocking", "requires_approval"]
    }
  },
  "check_command_status": {
    "description": "You can use this tool to get the status of a previously executed command by its Command ID ( non-blocking command ).",
    "params": {
      "type": "object",
      "properties": {
        "command_id": {"type": "string"},
        "wait_ms_before_check": {"type": "integer"},
        "output_character_count": {"type": "integer", "minimum": 0, "default": 1000},
        "skip_character_count": {"type": "integer", "minimum": 0, "default": 0},
        "output_priority": {"type": "string", "default": "bottom"}
      }
    }
  },
  "stop_command": {
    "description": "This tool allows you to terminate a currently running command( the command MUST be previously executed command. ).",
    "params": {
      "type": "object",
      "properties": {
        "command_id": {"type": "string"}
      },
      "required": ["command_id"]
    }
  },
  "open_preview": {
    "description": "You can use this tool to show the available preview URL to user if you have started a local server successfully in a previous toolcall, which user can open it in the browser.",
    "params": {
      "type": "object",
      "properties": {
        "preview_url": {"type": "string"},
        "command_id": {"type": "string"}
      },
      "required": ["preview_url", "command_id"]
    }
  },
  "web_search": {
    "description": "This tool can be used to search the internet, which should be used with caution, as frequent searches result in a bad user experience and excessive costs.",
    "params": {
      "type": "object",
      "properties": {
        "query": {"type": "string"},
        "num": {"type": "int32", "default": 5},
        "lr": {"type": "string"}
      },
      "required": ["query"]
    }
  },
  "finish": {
    "description": "The final tool of this session, when you think you have archived the goal of user requirement, you should use this tool to mark it as finish.",
    "params": {
      "type": "object",
      "properties": {
        "summary": {"type": "string"}
      },
      "required": ["summary"]
    }
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

This file contains 222 lines of json content.

### JSON Structure

This JSON file contains a dictionary with 16 top-level keys:

- `todo_write`: dict
- `search_codebase`: dict
- `search_by_regex`: dict
- `view_files`: dict
- `list_dir`: dict
- `write_to_file`: dict
- `update_file`: dict
- `edit_file_fast_apply`: dict
- `rename_file`: dict
- `delete_file`: dict
- `run_command`: dict
- `check_command_status`: dict
- `stop_command`: dict
- `open_preview`: dict
- `web_search`: dict
- `finish`: dict


## Usage and Examples

### How to Use

This JSON file can be loaded and used programmatically:

```python
import json

with open('Trae/Builder Tools.json', 'r') as f:
    data = json.load(f)
```


## Performance and Security Notes

### Performance Considerations
- File size: 8.07 KB
- Load time: negligible for files under 1MB
- JSON parsing overhead minimal for this file size


### Security Considerations
- No obvious security concerns detected
- Always review content before sharing publicly


## Related Files

- [`Chat Prompt.txt`](./Chat Prompt.txt_docs.md)
- [`Builder Prompt.txt`](./Builder Prompt.txt_docs.md)


## Testing and Validation

### How to Test
Validate JSON syntax:

```bash
python -c "import json; json.load(open('Trae/Builder Tools.json'))"
# or
jq . 'Trae/Builder Tools.json'
```


---

*Generated by Repo Book Generator v1.0.0 on 2025-11-15 09:46:00*
