# Week 2: Build an MCP Server for an External Service

Adapted from [Stanford CS146S Week 2](https://github.com/mihail911/modern-software-dev-assignments/blob/master/week2/assignment.md).

## Assignment overview

For this assignment, you will implement an MCP server that exposes at least three tools for a specific external service, allowing an agent to complete a useful workflow. Therefore, you choose **one service**, such as GitHub, Slack, or Google Calendar. For example, choose GitHub and implement:

| Your MCP tool | What it does |
| --- | --- |
| `find_issues(repository, query)` | Returns matching issues and their numbers. |
| `read_issue(repository, issue_number)` | Retrieves an issue's details. |
| `add_comment(repository, issue_number, body, dry_run)` | Previews or posts a comment. |

The tools must be composable; that is, the output returned by one tool must be the input of another. For example, the agent can find an issue, use its returned number to read it, and then comment on it. You implement these tool functions using the chosen service's existing API.

## Learning goals

- Expose a service through MCP and design tools that an agent can use correctly.
- Observe how schemas, descriptions, and errors influence tool use.
- Connect the server to an agent and demonstrate a working tool chain (i.e. how tools are composed within the workflow).

## Part I: Implement the server

Use [FastMCP](https://github.com/jlowin/fastmcp) to implement your integration:

- Provide at least three composable tools. 
- At least one write tool that changes real state.
- Keep the implementation in `assignment2/`, runs over stdio, and starts from a single documented command.

## Part II: Agent ergonomics and design principles

Note that the agent sees only your schemas and docstrings and nothing else. Therefore, consider the following principles when creating the tools from Part I:

- **Inputs:** Express constraints in schemas, e.g. enums for allowed values.
- **Outputs:** Return relevant fields and identifiers rather than raw API responses (which are also token-heavy).
- **Descriptions:** Explain each tool's purpose and where its input identifiers come from.
- **Errors:** Provide useful information about what failed and whether retrying or changing inputs could help.
- **Writes:** Provide a appropriate tool annotations, e.g. read only hints.

Try the tools with your agent. Briefly document one observed misuse and an improvement to the interface.

## Part III: Integrate, demonstrate, and test

- Connect the server to Claude Code.
- Capture one successful session: User prompt, at least two chained tool calls with arguments, and the result.
- Demonstrate one handled failure, including the error and the agent's next action.
- Include one test that invokes a tool through MCP, rather than directly calling its Python function. The external API may be mocked in this test.

Possible ways to provoke a failure—choose one, not all:

- Supply a nonexistent resource ID.
- Use a credential without permission for a write.
- Simulate a temporary API outage or rate-limit response.

Observe whether the agent corrects its input, retries appropriately, asks for help, or stops. Successful recovery is not always possible; the error should help the agent choose a sensible next action. Identify any simulated responses.

## Deliverables

Submit the following together in `assignment2/`:

1. **Server code and tests**, with startup and test commands.
2. **An example client configuration**, such as `.mcp.json.template` (note that this is a naming convention, claude code loads only `.mcp.json`): Show how to launch the server, using credential placeholders. Use your chosen client's format.
3. A completed **`assignment2/writeup.md`** with every `TODO` filled in.

## Submission instructions
Push the completed `writeup.md` file to your GitLab repository according to the submission guidelines (see the introductory slides).
