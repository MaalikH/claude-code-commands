# Create Product Spec

Generate a structured product specification markdown file from a description.

## User Input

```text
$ARGUMENTS
```

The user provides the full app/project details in `$ARGUMENTS`. This can be freeform text, bullet points, or a structured description.

## Execution Steps

### Step 1: Parse Input

Extract from the user's description:
- **Project Name**: The app/product name
- **Summary**: What it does (1-2 sentences)
- **Users**: Target user types
- **Features**: Key capabilities
- **Platforms**: Target platforms (web, iOS, macOS, etc.)
- **Architecture**: Technical components if mentioned
- **Non-Goals**: What's explicitly out of scope
- **Reference URL**: Any inspiration links mentioned

If critical info is missing (name or purpose), make a reasonable inference from context.

### Step 2: Generate Spec File

Create `<project-name-slugified>.md` in the current directory with:

```markdown
# [Project Name]

[Summary - 1-2 sentences describing what it does]

**Reference**: [URL if provided]

## Users

- [User type 1]
- [User type 2]

## Core Concepts

[If the user described domain concepts, entities, or key abstractions, list them here with brief definitions]

## Features

- [Feature 1]
- [Feature 2]

## Platforms

- **[Platform]**: [Brief description of role]

## Architecture

[If technical details provided, summarize components and their responsibilities]

## Non-Goals

- [Non-goal 1]
- [Non-goal 2]

## Story Generation Intent

Generate user stories that:
- Cover all listed features
- Address each user type's perspective
- Include acceptance criteria with Given/When/Then format
- Respect the defined non-goals
```

### Step 3: Save File

1. Write the file to the current directory
2. Report the file path

## Output

Report:
1. File path created
2. Next step: `/create-stories <filename>` to generate user stories
