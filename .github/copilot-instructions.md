# Copilot Instructions for FourXGame

This repository uses .NET 10.0 and follows strict documentation and testing practices.

## Build, Test, and Lint Commands

### Building
```bash
dotnet restore
dotnet build --no-restore /m:1 --configuration Release
```

### Testing
```bash
# Run all tests
dotnet test --no-build --configuration Release --collect:"XPlat Code Coverage" --logger trx

# Run a specific test
dotnet test --no-build --configuration Release --filter "FullyQualifiedName~YourTestNamespace.YourTestClass.YourTestMethod"
```

### Linting
```bash
# Verify C# formatting (doesn't change files)
dotnet format --verify-no-changes --verbosity diagnostic

# Fix C# formatting
dotnet format

# Check markdown links in docs/
pwsh -NoProfile -ExecutionPolicy Bypass -File scripts/check-doc-links.ps1

# Check markdown linting
markdownlint "**/*.md" --ignore "**/bin/**" --ignore "**/obj/**"
```

### Benchmarking
```bash
# Run benchmarks (project path needs to be configured)
dotnet run --no-build --project <path-to-benchmark-project> -- --filter * --baseline BenchmarkBaseline --fast
```

## Documentation Structure

This project has a rigorous documentation approach with templates and review processes.

### Documentation Templates
- **Feature Plans**: `docs/templates/plan-template.md` - Used for implementation plans
- **Technical Documentation**: `docs/templates/documentation-template.md` - Used for component/feature documentation

### Documentation Principles
1. **Plain English**: All documentation must be in plain English
2. **Define Non-Plain Terms**: Acronyms (e.g., BFS), mathematical terms (e.g., Shannon entropy), or domain-specific terms must be defined in a "Definition of Terms" section before use
3. **Well-Formed Structure**: Documents must follow templates and include:
   - Overview with purpose and use cases
   - Table of contents
   - Definition of terms (alphabetically sorted)
   - Technical details with diagrams (ASCII art, mermaid, or KaTeX math)
   - Examples with minimal, compile-ready code snippets including XML docs
   - See Also section for related documentation
   - References for external resources

### Documentation Organization
- Feature documentation: `docs/features/<feature-name>.md`
- Child features: `docs/features/<feature-name>/<child-feature>.md`
- Implementation plans: `docs/plans/<plan-name>.md`
- Child plans: `docs/plans/<plan-name>/<child-plan>.md`

### Link Rules for Documentation
- Use **relative paths only** (no absolute paths like `C:\` or root-anchored paths like `/`)
- Links must stay within the repository root
- External links (http://, https://, mailto:) are allowed
- No `file://` or `vscode://` schemes

## Custom Agents and Skills

This repository has custom GitHub Copilot agents configured in `.github/agents/`:

1. **FeaturePlanner** (`feature-plan.agent.md`) - Generates implementation plans for new features
2. **FeatureImplementer** (`feature-implement.agent.md`) - Implements planned features
3. **ImplementationDocumenter** (`implementation-doc.md`) - Documents existing code implementations

Available skills in `.github/skills/`:
- `feature-doc-elaborate` - Expand feature documentation
- `feature-doc-review` - Review feature documentation against standards
- `feature-implement` - Implement features from plans
- `implementation-doc-update` - Update implementation documentation
- `implementation-doc-review` - Review implementation documentation
- `gh-cli` - Reference for using GitHub CLI to manage repos, issues, PRs, Actions, and releases
- `github-issues` - Create and manage GitHub issues with labels, assignees, and milestones
- `microsoft-code-reference` - Look up Microsoft API references and find working code samples
- `microsoft-docs` - Query official Microsoft documentation for .NET, Azure, and other technologies
- `nuget-manager` - Safely manage NuGet packages using `dotnet` CLI with strict workflows
- `prd` - Generate comprehensive Product Requirements Documents with user stories and specs
- `refactor` - Perform surgical code refactoring to improve maintainability without changing behavior

## Code Conventions

### C# Style
- **Nullable reference types**: Enabled globally (`<Nullable>enable</Nullable>`)
- **Analysis**: All analyzers enabled with latest analysis level
- **NuGet auditing**: Enabled for all packages with low severity threshold
- **Indentation**: 2 spaces (configured in `.editorconfig`)
- **Line endings**: CRLF
- **Charset**: UTF-8 with BOM
- **Var usage**: Prefer `var` for built-in types, obvious types, and elsewhere (suggestion level)
- **Modifiers order**: public, private, protected, internal, new, abstract, virtual, sealed, override, static, readonly, extern, unsafe, volatile, async

### Code Analysis Suppressions
- `CA1303` (Localization) - Silenced
- `CA1805` (Unnecessary initialization) - Silenced
- `CA5394` (Insecure randomness) - Disabled
- `CA1707` (Identifiers should not contain underscores) - Disabled

## Versioning

Uses **GitVersion** for semantic versioning:
- Main branch: Auto-increments patch version
- Feature branches: `features/<name>` or `feature/<name>`
- Release branches: `releases/<name>` or `release/<name>`
- PR branches: `pull-requests/<number>`, `pull/<number>`, or `pr/<number>`

### Version Control via Commit Messages
- `+semver: major` or `+semver: breaking` - Bump major version
- `+semver: minor` or `+semver: feature` - Bump minor version
- `+semver: patch` or `+semver: fix` - Bump patch version
- `+semver: none` or `+semver: skip` - No version bump

## CI/CD

GitHub Actions workflow (`.github/workflows/main.yml`) runs on push and PR:
1. **Build** - Compiles project, runs on Windows
2. **Test** - Runs unit tests with code coverage
3. **Lint** - Verifies C# formatting compliance
4. **Benchmark** - Runs performance benchmarks
5. **CodeQL** - Static security analysis
6. **docs-links** - Validates documentation links
7. **markdownlint** - Validates markdown formatting
8. **pr-size** - Enforces PR limits (max 1000 lines changed, max 50 files)

Main branch commits are automatically tagged with semantic version.

## Requirements Documentation

Features use Given/When/Then syntax for requirements:
```markdown
- (***requirement status***) Requirement descriptive text
  - GIVEN a precondition
  - WHEN an event happens
  - THEN an action is taken / a result property is true

> Details of missing implementation / link to phase with missing implementation
```

Requirement status examples: COMPLETE, IN PROGRESS, BLOCKED, etc.

## Code Examples in Documentation

All code examples must:
- Be minimal and compile-ready
- Include XML documentation comments
- Include file path and parent context as comments
- Highlight performance considerations where relevant

Example format:
```csharp
// path/to/file.cs
// parent class or method name

/// <summary>
/// Brief description of what the method does.
/// </summary>
/// <remarks>
/// Performance notes or important constraints.
/// </remarks>
public ReturnType MethodName(/* params */)
{
  // implementation
}
```
