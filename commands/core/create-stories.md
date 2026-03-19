# Generate User Stories

Run the SpecKit user story generator on a product specification file.

## User Input

```text
$ARGUMENTS
```

`$ARGUMENTS` should be the path to a spec markdown file (e.g., `my-app.md`).

If `$ARGUMENTS` is empty, look for `.md` files in the current directory and ask the user which one to use.

## Execution Steps

### Step 1: Validate Input File

1. If no file specified, list available `.md` files and ask user to choose
2. Verify the file exists
3. Read the file to confirm it looks like a product spec (has sections like Features, Users, etc.)

### Step 2: Run Story Generator

Execute the story generator:

```bash
cd /Users/maalikahmadtech/Dev/spec-generate && npx ts-node src/cli.ts -i "<absolute-path-to-spec-file>" -o "<spec-file-basename>-stories.md" --no-clarify
```

**Note**: The generator uses OpenAI API. Ensure `OPENAI_API_KEY` is set in the environment.

### Step 3: Handle Output

1. Wait for generation to complete (may take 30-60 seconds)
2. If successful, read the generated stories file
3. Display a summary:
   - Number of epics generated
   - Number of user stories
   - Story priorities breakdown (P1/P2/P3)

### Step 4: Review Stories

1. Show the user the generated stories file path
2. Offer to open the file for review
3. Ask if they want to regenerate with different parameters

## Error Handling

- **Missing OPENAI_API_KEY**: Inform user to set the environment variable
- **File not found**: List available .md files and ask user to choose
- **Generation fails**: Show error message and suggest checking the spec format

## Output

Report:
1. Input spec file used
2. Output stories file path
3. Generation statistics (epics, stories, priorities)
4. Any warnings from the generator
