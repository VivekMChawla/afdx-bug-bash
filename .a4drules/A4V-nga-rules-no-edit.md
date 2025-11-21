Title: Agentforce .agent Authoring and Review Rules

Scope
- Applies to all files with extension .agent located under force-app/**/aiAuthoringBundles/**.
- Covers reading, validating, editing, and generating Agentforce agent definitions using the AF Authoring DSL.

General Guidance
- Treat .agent as YAML-like AF Authoring DSL. Preserve indentation and structure.
- Whitespace and indentation are significant;  Four spaces per indent level.
- Do not introduce secrets, real user emails, or environment-specific identifiers directly in .agent files. Use placeholders or variables and document required runtime bindings separately.
- Keep agent branding consistent across:
  - system.instructions
  - config.agent_name, config.developer_name, config.agent_description
  - folder and file names (bundle directory should reflect the agent brand/purpose)

Required Sections
- system:
  - instructions must be single-source-of-truth for agent persona and boundaries.
  - Keep concise, brand-aligned, and safe (disallow PII handling unless explicitly intended).
- config:
  - agent_name, agent_type, developer_name are mandatory.
  - default_agent_user must be a placeholder in source control (e.g., <EMAIL_ADDRESS_PLACEHOLDER>) and set during deployment.
  - user_locale must be valid (e.g., en_US).
  - enable_enhanced_event_logs should default to true in non-prod unless specified otherwise.
- variables:
  - Declare all variables referenced by any topic/transition/actions.
  - Provide type, default, and description.
  - Only include variables used by transitions or actions; remove dead variables.
- topics:
  - Each topic requires description, reasoning_instructions (>> block), and reasoning_actions.
  - Use @utils.transition for navigation and ensure target topic names exist.
  - Guard transitions with available when conditions tied to declared variables.
- actions:
  - Use explicit targets with one of:
    - apex://ClassOrNamespace.methodOrIdentifier
    - flow://FlowApiName or flow://omni-flow-<name>
    - http(s):// for future extensibility (if supported)
  - Define inputs/outputs as they are expected at runtime. Inputs and outputs must match the Apex or Flow contracts.

Validation Checklist (pre-commit)
- Branding:
  - Folder name and file name match config.agent_name intent.
  - system.instructions, agent_description reference the right brand.
- Dependencies:
  - All apex:// targets correspond to Apex classes in force-app or are planned dependencies.
  - All flow:// targets correspond to existing Flows or are stubs with specs.
- Variables:
  - No undefined variables used in available when conditions.
  - No unused variables present.
  - After being initialized, variables are ALWAYS referenced as `@variable.<variableName>` when getting and setting values.
- Transitions:
  - All @utils.transition references point to real topics.
  - No circular or dead-end routes unless intentional; ensure at least one path to end_interaction.
- Reasoning blocks:
  - Use knowledge/tooling as single source of truth where stated.
  - Avoid open-ended actions without guard rails; add clarifying-question guidance where appropriate.
- before_reasoning blocks:
  - DO NOT add `before_reasoning` blocks unless specifically requested.
  - `before_reasoning` blocks must appear after a topic's `actions` block and before its `reasoning_instructions` block.
  - Actions referenced in `before_reasoning` blocks are preceded by the `run` keyword.
  - The `run` keyword starts a block. Keywords that are part of the `run` block must be indented on their own lines.
  - Actions that require input use the `with` keyword to pass values.
  - Action ouput must be assigned to a variable using the `set` keyword for that value to be used in the `Reasoning` block that follows.
- Security/PII:
  - No real emails/usernames; use placeholders.
  - Do not embed API keys or secrets.
- Linting:
  - Validate YAML-like indentation; four spaces per indent level.
  - Wrap long strings in quotes when they contain special characters (:, #, @).
  - Keep comments with a single # at start-of-line; avoid inline # inside values.

Authoring Conventions
- Topic naming: use kebab_case or snake_case consistently; avoid spaces.
- Action naming: verb_noun style (e.g., get_order_info, set_order_id).
- Transition alias naming: describe outcome clearly (e.g., go_to_orders, mark_completion).
- Put start agent entry as start_agent topic_selector or similarly named root router.
- Use before_reasoning to hydrate variables from actions (e.g., check_business_hours) before gating transitions.
- Prefer explicit available when conditions on transitions instead of relying on implicit behavior.
- Keep reasoning_instructions in imperative tone; use bullet lists sparingly.

Testing and Evaluation
- For each .agent, include or update an aiEvaluationDefinitions test set that:
  - Exercises topic selection paths from common user intents.
  - Validates guarded transitions (e.g., business hours, order cancellation window).
  - Verifies end_interaction completion and rating capture flows.
- If adding new apex:// or flow:// actions, provide minimal Apex test stubs or Flow definitions to avoid runtime gaps.
- Prefer running evaluations in scratch orgs or sandboxes; do not run against production by default.

Change Management
- When renaming or rebranding agents, update:
  - Directory name under aiAuthoringBundles
  - *.agent filename
  - config fields and system.instructions
  - Any aiEvaluationDefinitions and aiPlanners referencing the agent
- Record a short changelog entry in the commit message describing behavior-impacting changes (topics added/removed, transitions, guards, or external dependencies).

Safe Defaults
- default_agent_user: "<AGENT_USER_PLACEHOLDER>"
- user_locale: "en_US"
- enable_enhanced_event_logs: true
- Include an Off_Topic topic that guides users back to supported intents.
- Provide an end_interaction topic with a rating action if conversation quality needs measurement.

Common Pitfalls and How to Avoid Them
- Mismatch between folder name and brand: align before commit.
- Undefined apex/flow targets: add stubs or update targets to existing assets.
- Variables referenced but not declared: run through the validation checklist.
- Over-permissive escalation: always gate escalation by necessary context (e.g., business_hours, has order_id).
- Free-form knowledge answers: constrain to knowledge base or tools as appropriate; avoid hallucinations.

Repository Integration
- Place agent bundles under force-app/main/default/aiAuthoringBundles/<AgentName>/
- Commit companion metadata files (e.g., aiAuthoringBundle-meta.xml) in the same folder per Salesforce metadata packaging standards.
- Ensure manifests/package.xml or dedicated manifests include aiAuthoringBundles when deploying with sf or MCP deploy_metadata.

Deployment Notes
- Use MCP deploy_metadata tool before raw CLI commands.
- Never hardcode org-specific usernames or emails inside the .agent file; inject via environment or post-deploy scripts.
- For CI: add a validation job that greps for apex:// and flow:// targets and cross-checks their existence.

By following this rule file, Dev Agent will read, validate, and safely modify .agent definitions, keep branding consistent, verify dependencies, and avoid embedding sensitive data in source control.