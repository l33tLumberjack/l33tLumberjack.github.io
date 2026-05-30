## Instructions for Creating Blog Posts from Templates

This directory contains the workflow for generating new blog posts using an opinionated, editorial style. When tasked with creating a new post:

### 1. Initialization & Setup
- **Scaffold**: Copy `template.md` into a new directory within `projects/l33tLumberjack.github.io/Blog/[Topic-Name]/`.
- **Rename**: Rename the copy to `index.md`.
- **Metadata**: Update the YAML frontmatter with a descriptive `title` and the appropriate `category` (e.g., `blue`, `red`, or `interview`).

### 2. Content Development (Iterative Process)
- **Question Generation**: Populate the `## Questions` section with technical, topic-specific queries.
- **Technical Drafting**: Answer each question sequentially. Treat the Q&A section as a drafting scaffold, not the final published format.
- **Command Accuracy**: Test command-line examples and configuration snippets when practical. If a command has not been tested, avoid presenting it as guaranteed.
- **Synthesis**: Review the Q&A to ensure a logical progression of ideas.

### 3. Editorial Transformation (The "Opinionated" Layer)
Transform the raw Q&A into an editorial narrative:
- **Tone**: Adopt a conversational, professional, yet slightly cynical tone. Critique industry "anti-patterns."
- **Contrast Pattern**: Use code blocks to demonstrate "Bad Practice" (e.g., insecure TLS settings) followed immediately by "Best Practice" (the corrected configuration).
- **Thematic Integration**: Connect technical details to broader security concepts like attack surface reduction, lateral movement, or defense-in-depth.

### 4. Finalization & Quality Control
- **Structure Check**: Ensure the flow follows: `Introduction` $\rightarrow$ `Problem/Anti-pattern` $\rightarrow$ `Technical Analysis` $\rightarrow$ `Solution/Best Practice` $\rightarrow$ `Conclusion`.
- **Disclaimer**: Verify that the `## Legal Disclaimer` is present and unmodified.
- **Validation**: Confirm all Markdown syntax is valid and that any referenced files or tools are correctly documented.
- **Generated Output**: Do not edit generated output directories such as root `Blog/` or `build/site/` directly.
