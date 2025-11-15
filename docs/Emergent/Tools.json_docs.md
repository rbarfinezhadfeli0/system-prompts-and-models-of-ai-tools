# Documentation: Tools.json

## File Metadata

- **Path**: `Emergent/Tools.json`
- **Size**: 7880 bytes (7.70 KB)
- **MIME Type**: application/json
- **Lines**: 233
- **Words**: 844
- **Last Modified**: 2025-11-15T09:41:05.383333

## Original Source

```json
AVAILABLE TOOLS AND THEIR JSON SCHEMAS:

1. mcp_bulk_file_writer
   Description: Write multiple files simultaneously for improved performance
   Parameters:
   {
     "files": {
       "type": "array",
       "items": {
         "type": "object",
         "properties": {
           "path": {"type": "string", "description": "Absolute path to the file"},
           "content": {"type": "string", "description": "Raw text content for the file"}
         },
         "required": ["path", "content"]
       }
     },
     "capture_logs_backend": {"type": "boolean", "default": false},
     "capture_logs_frontend": {"type": "boolean", "default": false},
     "status": {"type": "boolean", "default": false}
   }

2. emergent_integrations_manager
   Description: Get the Emergent LLM key for llm integration (openai, anthropic, google)
   Parameters: {}

3. extract_file_tool
   Description: Extract specific structured data from document files
   Supported formats: .txt, .js, .py, .html, .css, .json, .xml, .csv, .md, .pdf, .docx, .xlsx, .pptx, .png, .jpg, .jpeg, .webp, .heic, .heif, .wav, .mp3, .mpeg, .aiff, .aac, .ogg, .flac, .mov, .mpeg, .mp4, .mpg, .avi, .wmv, .mpegps, .flv
   Parameters:
   {
     "source": {"type": "string", "description": "Direct URL or local file path"},
     "prompt": {"type": "string", "description": "What specific data to extract"},
     "headers": {"type": "object", "description": "Additional HTTP headers (optional)"},
     "timeout": {"type": "integer", "default": 30}
   }

4. ask_human
   Description: Ask human user for clarification, additional info, confirmation, or feedback
   Parameters:
   {
     "question": {"type": "string", "description": "The question to ask from human"}
   }

5. analyze_file_tool
   Description: AI-powered analysis on document files for insights and patterns
   Supported formats: Same as extract_file_tool
   Parameters:
   {
     "source": {"type": "string"},
     "analysis_type": {"type": "string", "enum": ["general", "structure", "content", "sentiment", "security", "performance", "compliance", "custom"]},
     "query": {"type": "string", "description": "Specific analysis question (optional)"},
     "headers": {"type": "object"},
     "timeout": {"type": "integer", "default": 30}
   }

6. mcp_glob_files
   Description: Fast file pattern matching with glob patterns
   Parameters:
   {
     "pattern": {"type": "string", "description": "The glob pattern to match files against"},
     "path": {"type": "string", "description": "Directory to search in (optional)"}
   }

7. execute_bash
   Description: Execute a bash command in the terminal
   Parameters:
   {
     "command": {"type": "string", "description": "The bash command to execute"}
   }

8. grep_tool
   Description: Search file contents using ripgrep with regex patterns
   Parameters:
   {
     "pattern": {"type": "string", "description": "The regex pattern to search for"},
     "path": {"type": "string", "description": "Directory or file to search in"},
     "case_sensitive": {"type": "boolean"},
     "context_lines": {"type": "integer"},
     "include": {"type": "string", "description": "File patterns to include"}
   }

9. mcp_view_file
   Description: View file or directory contents
   Parameters:
   {
     "path": {"type": "string", "description": "The absolute path to the file to view"},
     "view_range": {"type": "array", "items": {"type": "integer"}, "description": "Optional line range [start, end]"}
   }

10. mcp_search_replace
    Description: Search and replace exact string in file
    Parameters:
    {
      "path": {"type": "string"},
      "old_str": {"type": "string", "description": "Exact string to replace - must match EXACTLY"},
      "new_str": {"type": "string", "description": "Replacement string"},
      "replace_all": {"type": "boolean", "default": false},
      "run_lint": {"type": "boolean", "default": false},
      "status": {"type": "boolean", "default": false}
    }

11. mcp_lint_python
    Description: Python linting using ruff
    Parameters:
    {
      "path_pattern": {"type": "string", "description": "File/directory path or glob pattern"},
      "fix": {"type": "boolean", "default": false},
      "exclude_patterns": {"type": "array", "items": {"type": "string"}}
    }

12. mcp_lint_javascript
    Description: JavaScript/TypeScript linting using ESLint
    Parameters:
    {
      "path_pattern": {"type": "string"},
      "fix": {"type": "boolean", "default": false},
      "exclude_patterns": {"type": "array", "items": {"type": "string"}}
    }

13. mcp_create_file
    Description: Create a new file with specified content
    Parameters:
    {
      "path": {"type": "string", "description": "The absolute path for the new file"},
      "file_text": {"type": "string", "description": "Content for the new file"},
      "run_lint": {"type": "boolean", "default": false}
    }

14. mcp_insert_text
    Description: Insert text at a specific line number in a file
    Parameters:
    {
      "path": {"type": "string"},
      "new_str": {"type": "string"},
      "insert_line": {"type": "integer", "minimum": 0},
      "run_lint": {"type": "boolean", "default": false}
    }

15. finish
    Description: Provide concise summary for clarity and handoff
    Parameters:
    {
      "summary": {"type": "string", "description": "Provide summary based on given inputs and examples"}
    }

16. get_assets_tool
    Description: Retrieve attached assets from the database for the current job/run
    Parameters: {}

17. screenshot_tool
    Description: Execute screenshot commands using Playwright
    Parameters:
    {
      "page_url": {"type": "string"},
      "script": {"type": "string", "description": "Complete Python Playwright script"},
      "capture_logs": {"type": "boolean", "default": false}
    }

18. mcp_view_bulk
    Description: View multiple files or directories in sequence
    Parameters:
    {
      "paths": {"type": "array", "items": {"type": "string"}, "minItems": 1, "maxItems": 20}
    }

19. web_search_tool_v2
    Description: Search the web for current information, recent events, or topics
    Parameters:
    {
      "query": {"type": "string"},
      "search_context_size": {"type": "string", "enum": ["low", "medium", "high"]}
    }

20. think
    Description: Think about something - append thought to log
    Parameters:
    {
      "thought": {"type": "string"}
    }

21. crawl_tool
    Description: Scrape, crawl, retrieve, fetch or extract complete content from webpages
    Parameters:
    {
      "url": {"type": "string"},
      "extraction_method": {"type": "string", "enum": ["scrape"]},
      "formats": {"type": "string", "enum": ["html", "markdown", "json"], "default": "markdown"},
      "question": {"type": "string", "default": "text"}
    }

22. vision_expert_agent
    Description: AI-powered assistant for selecting and returning relevant image URLs
    Parameters:
    {
      "task": {"type": "string", "description": "Detailed task for the skilled agent to perform"}
    }

23. auto_frontend_testing_agent
    Description: Expert agent for UI testing using playwright and browser automation
    Parameters:
    {
      "task": {"type": "string"}
    }

24. deep_testing_backend_v2
    Description: Expert agent for testing backend using curl and UI using playwright
    Parameters:
    {
      "task": {"type": "string"}
    }

25. integration_playbook_expert_v2
    Description: Creates comprehensive playbooks for integrating third-party APIs and services
    Parameters:
    {
      "task": {"type": "string"}
    }

26. support_agent
    Description: Help with any answers about the Emergent platform
    Parameters:
    {
      "task": {"type": "string"}
    }

27. deployment_agent
    Description: Expert agent to debug native deployment issues on Emergent
    Parameters:
    {
      "task": {"type": "string"}
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

This file contains 233 lines of json content.

### Structure

JSON parsing failed. See original source above.


## Usage and Examples

### How to Use

This JSON file can be loaded and used programmatically:

```python
import json

with open('Emergent/Tools.json', 'r') as f:
    data = json.load(f)
```


## Performance and Security Notes

### Performance Considerations
- File size: 7.70 KB
- Load time: negligible for files under 1MB
- JSON parsing overhead minimal for this file size


### Security Considerations
- No obvious security concerns detected
- Always review content before sharing publicly


## Related Files

- [`Prompt.txt`](./Prompt.txt_docs.md)


## Testing and Validation

### How to Test
Validate JSON syntax:

```bash
python -c "import json; json.load(open('Emergent/Tools.json'))"
# or
jq . 'Emergent/Tools.json'
```


---

*Generated by Repo Book Generator v1.0.0 on 2025-11-15 09:46:00*
