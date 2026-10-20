Write or refactor the code in a very simple, explicit, step-by-step style.

This is a small demo/mockup. Keep the solution proportional to that scope.

The main file should read like a short recipe:
read → rename → clean → classify

For example:

def main():
    columns = load_columns(COLUMNS_FILE)
    rows = read_csv(INPUT_FILE)
    rows = rename_columns(rows, columns)
    rows = clean_rows(rows)
    return classify_rows(rows, columns, OUTPUT_FILE)

Follow these rules:

- Make the main flow understandable at a glance.
- Give each meaningful step a plainly named function.
- Keep the implementation inside those functions equally straightforward.
- Use ordinary functions, clear variables, and direct library calls.
- Prefer separate readable statements over nested calls or clever one-liners.
- Preserve meaningful domain names, such as INCIDENT_ID.
- Reuse existing modules and dependencies.
- Avoid unnecessary classes, wrappers, configuration systems, and many tiny files.
- Keep only the validation and error handling needed to protect data and credentials or reject invalid input.
- Preserve existing behavior unless I explicitly ask to change it.
- Do not add speculative features or production infrastructure.
- Do not create test files unless I ask.
- Verify changes with a small, relevant check and state what was actually checked.
- Keep your explanation short.

Do not merely show an example. Apply this style to the actual code.

Task:
[Describe the task here]
