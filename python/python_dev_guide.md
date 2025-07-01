# Python Development Guide: Standards, Tooling, and Workflow

This file provides essential guidelines and best practices for developers contributing to Python projects. 
Our aim is to foster a collaborative environment with high standards for code quality, maintainability, and efficiency.

## 1. Setting Up Your Development Environment & Workflow

This project utilizes `uv` for efficient dependency management and environment setup. Assuming `uv` is already installed and the project/repository created using `uv init`, follow these steps to get started and maintain your development workflow:

* Initialize Environment & Install Dependencies. Use `uv sync`. This command will automatically create a virtual environment (typically in a `.venv` directory in the project root) and install all dependencies specified in `pyproject.toml`.
* Running Code. Use `uv run [project app command]`. Examples: `uv run fastapi dev main.py`, `uv run streamlit run app.py`.

* Manage project dependencies with `uv`. 
    * Add a new dependency: `uv add <package-name>`
    * Add a development-only dependency: `uv add --dev <package-name>`
    * Remove a dependency: `uv remove <package-name>`
    * Update all dependencies: `uv update`

* Only add dependencies you really need at the moment for the app. Do not add dependencies in advance. Always ask before adding new dependencies.

## 2. Python Project Structure

Understanding the project's layout is crucial for effective contribution. A typical Python project structure often looks like this:

.
├── main.py                       # Main application entry point
├── src/                          # Source code for the application
│   ├── __init__.py               # Makes 'src' a Python package
│   └── [source files]            # Appropriate folders and files structure for the app
├── tests/                        # Unit and integration tests
│   ├── __init__.py
│   ├── test_main.py
│   └── ...
├── docs/                         # Project documentation (user guide, API reference)
│   ├── usage.md                  # Example: File with info how to use the project
│   └── ...                       
├── data/                         # Data files (e.g., pre-defined prompts, example data)
│   └── example_data.json
├── .gitignore                    # Specifies files/directories to ignore in Git
├── .env.example                  # Example of environment variables (without sensitive data)
├── pyproject.toml                # Project metadata, dependencies, and tool configurations (e.g., ruff, pytest)
├── README.md                     # General project overview and quick start guide
├── CONTRIBUTING.md               # Detailed contribution guidelines
├── CODE_OF_CONDUCT.md            # Community code of conduct
├── GEMINI.md                     # This detailed development guide
└── PLAN.md                       # Development plan specific for this project
 
### Flexible Project Structure

**Standard Web API Project:**
src/
├── api/           # API routes and endpoints
├── core/          # Business logic, domain models
├── db/            # Database models, migrations
├── services/      # External service integrations
└── utils/         # Shared utilities

**CLI Tool Project:**
src/
├── commands/      # CLI command implementations
├── core/          # Core functionality
└── utils/         # Shared utilities

**Data Processing Project:**
src/
├── processors/    # Data transformation logic
├── models/        # Data models and schemas
├── io/           # Input/output handlers
└── utils/         # Shared utilities

Adapt structure to your domain - the key is logical separation and clear naming.

## 3. Preferred Libraries

Prefer the use of the following preferred libraries to ensure high quality, maintainability, and efficient development:

* **`fastapi`**: For rapidly building high-performance asynchronous APIs. To ensure all standard dependencies are included, install it using `uv add fastapi[standard]`.
* **`pydantic`**: For data validation, parsing, and settings management, ensuring data integrity.
* **`sqlmodel`**: A powerful SQL toolkit and Object Relational Mapper (ORM) for robust database interactions.
* **`pydantic-ai`**: Specifically for structured output from AI models, facilitating reliable parsing of generative AI responses.
* **`rich`**: For generating beautiful and informative terminal output, including syntax highlighting, tables, and progress bars.
* **`typer`**: For building intuitive and powerful command-line interfaces (CLIs) with minimal code, built on top of `Click`. 
* **`pydantic-settings`**: Manage configs and settings

## 4. Source Code Standards

We maintain high standards for code quality to ensure maintainability, reliability, and ease of collaboration.

### 4.1 Coding Style

- Follow consistent Python style and structure for readable, maintainable code.
- Adhere to PEP 8 for naming, formatting, and layout
- Use ruff to automatically enforce style
- All new code must include type hints
- Avoid overly clever constructs – optimize for clarity

### 4.2 Source Documentation

- Use Google-style docstrings for public functions, classes, and modules
- Use inline comments to explain why, not what
- For simple functions, type hints may be sufficient without full docstrings

* **Docstrings:** All functions, classes, and modules should have clear, concise docstrings following the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html#s3.8-comments-and-docstrings).
* **Inline Comments:** Use comments sparingly to explain *why* complex logic is implemented, not *what* it does.
 
**Docstring Requirements:**
- **Public APIs**: Always require comprehensive docstrings (classes, public methods, functions)
- **Private/Internal**: Brief docstrings for complex logic only
- **Simple utilities**: Type hints sufficient, docstring optional

**Format Examples:**
``` python
# Required - Public API
def process_user_data(data: dict[str, Any]) -> UserModel:
    """Process raw user data into validated model.
    
    Args:
        data: Raw user data from external source
        
    Returns:
        Validated user model instance
        
    Raises:
        ValidationError: If data fails validation
    """

# Optional - Simple utility  
def normalize_email(email: str) -> str:
    """Convert email to lowercase and strip whitespace."""
    return email.lower().strip()

# Not required - obvious function
def get_user_id(user: User) -> int:
    return user.id
```

### 4.3 Tooling and Quality Checks

- `uv` as a fast and reliable package manager to ensure deterministic and reproducible environments. 
- `pytest` for running tests, ensuring our code is reliable and changes don't introduce regressions. 
- `ruff` is our linter and formatter, enforcing code style and catching common issues quickly.
- `ty` supports robust type-checking and type annotation, helping us maintain code clarity and correctness. 
- `bandit`: For identifying common security vulnerabilities, Bandit helps us maintain a secure codebase.

Together, these tools help us maintain high code quality and follow best practices efficiently.

If any required tools (`ruff`, `pytest`, `ty`, etc.) are missing from your local environment, install them as **development dependencies** using:
```bash
uv add --dev pytest ruff ty bandit
```

Do not add these tools to the production dependencies.

### 4.4. Code Formatting and Linting using ruff 

*   `ruff` is our linter and formatter, enforcing code style and catching common issues quickly. It is configured to run automatically via `pre-commit` hooks.
*   You can also run `ruff` manually with:
    ```bash
    uv run ruff check --fix .   # Automatically fixes linting issues
    uv run ruff format .        # Formats code according to Black-like style
    ```

### 4.5. Type Hinting & Static Analysis

*   **All new code must include type hints.**
*   Existing code should be updated with type hints as part of refactoring or feature work.
*   Type checking is performed automatically via `pre-commit` hooks using `mypy` (or `ty` if configured).
*   You can also run `ty` manually to check type correctness (add `ty` as a dev dependency via `uv add --dev ty`):
    ```bash
    uv run ty check src/
    ```

### 4.6. Pre-commit Hooks for Automated Checks

To ensure consistent code quality and adherence to our standards, we utilize `pre-commit` hooks. These hooks automatically run checks (like linting, type-checking, and security analysis) before you commit your changes, preventing common issues from entering the codebase.

**Setup:**

1.  **Install `pre-commit`:** If you don't have it already, install `pre-commit` as a development dependency:
    ```bash
    uv add --dev pre-commit
    ```
2.  **Configure Hooks:** Create a file named `.pre-commit-config.yaml` in the root of your project with the following content. This configuration tells `pre-commit` which tools to run and how to run them.

- ruff: see https://github.com/astral-sh/ruff-pre-commit for how to setup

3.  **Install Hooks into your Git Repository:**
    ```bash
    uv run pre-commit install
    ```

**Usage:**

Once installed, `pre-commit` hooks will automatically run every time you attempt to `git commit`. If any checks fail, the commit will be aborted, and you'll need to fix the issues before you can commit. You can also manually run all checks at any time with:

```bash
uv run pre-commit run --all-files
```

## 5. Testing

* All new features and bug fixes **must** be accompanied by comprehensive tests.

**Philosophy: Test-First Development**
- **Primary verification**: Tests should be your main way to verify functionality, not manual app testing
- **Fast feedback loop**: Running tests should be faster than starting the app
- **Confidence in changes**: Comprehensive tests allow safe refactoring and feature additions
- **Documentation**: Tests serve as living documentation of expected behavior

**Test Structure and Organization**
tests/
├── conftest.py              # Shared fixtures and configuration
├── unit/                    # Fast, isolated unit tests
├── integration/             # Integration tests with external dependencies
├── e2e/                     # End-to-end tests
└── fixtures/                # Test data and fixtures
└── data/                     # sample_data.json, mock_responses.py etc...

* We use `pytest` for our test suite where applicable.
* Install pytest-related dependencies as needed (e.g pytest-cov)
* Aim for at least 90% test coverage where practical. Use pytest-cov to measure.
* Tests should be fast, isolated, and deterministic. Use mocking where appropriate to isolate tests from external dependencies.

**Test-Driven Development Workflow**
- Write failing test first: Define expected behavior
- Run test: Confirm it fails for the right reason
- Write minimal code: Make test pass
- Refactor: Improve code while keeping tests green
- Repeat: For each new feature or bug fix

**When Manual Testing is Acceptable**
- UI/UX validation (visual appearance, user experience)
- Integration with external systems during initial setup
- Performance testing with real-world data volumes
- Accessibility testing with screen readers
- Browser compatibility testing

**Never Manual Test These**
- Business logic validation
- API endpoint behavior
- Data validation and transformation
- Error handling and edge cases
- Configuration and environment handling

### Testing FastAPI Applications

For FastAPI applications, we use `TestClient` for robust and isolated testing. This allows you to simulate requests to your application without actually running a live server.

### Database State Management for Tests

When testing endpoints that interact with the database, it's crucial to ensure a clean and consistent database state for each test. We achieve this using `pytest` fixtures to drop and recreate tables before each test run.

## 6. Security

### 6.1. Secrets and Configuration Management

Proper secrets and environment configuration handling is essential for security and portability. This project follows the 12-factor app principle of separating config from code using environment variables.

#### Tools

* **[`pydantic-settings`](https://docs.pydantic.dev/latest/integrations/settings/)**: Used to define and load application settings from environment variables (and `.env` files).
* **`.env` / `.env.example`**: Local file for non-secret config and local development settings.

#### Guidelines

**✅ Define settings with `pydantic-settings`**

* Create a central settings module (e.g. `src/core/settings.py`):
* Use `settings` throughout your application to access values in a strongly-typed and validated way.

**✅ Use `.env` for local secrets**

* Never commit actual secrets to `.env`. Use it **locally only**.
* Share structure and required variables via `.env.example`.

**✅ Git Hygiene**

Ensure `.env` is present in your `.gitignore`. This prevents accidental check-in of sensitive configuration.

#### Tips

* Load different `.env` files for different environments using `pydnatic-settings`, `python-dotenv` or a wrapper script, if needed.
* For production, pass secrets via actual environment variables (e.g. in Docker or CI).
* Avoid placing long-lived secrets in source control or long-lived `.env` files—use secret managers for production (e.g. AWS Secrets Manager, Vault, or Doppler).

### 6.2 Security Analysis

*   We use `Bandit` to identify common security vulnerabilities in our Python code. It is configured to run automatically via `pre-commit` hooks.
*   You can also run `Bandit` manually:
    ```bash
    uv add --dev bandit # Only needed once to add bandit as a dev dependency
    uv run bandit -r src/
    ```
    *Review any reported issues and address them as necessary. False positives can be marked for exclusion in a `.bandit` file or with inline comments.*

## 7. Documentation

* **User Documentation:** Any changes to user-facing functionality should be reflected in the `docs/` directory.

## 6. When to Deviate from Guidelines

These guidelines optimize for most scenarios but aren't absolute rules:

**Acceptable Deviations:**
- **Performance-critical code**: Skip type checking if it impacts hot paths
- **Prototype/spike work**: Defer testing and documentation until direction is clear
- **Legacy integration**: Use required libraries even if not on preferred list
- **External constraints**: Client/platform requirements override preferences
- **Emergency fixes**: Skip full review process for critical production issues

**Deviation Process:**
1. Document reason in commit message or PR description
2. Create follow-up issue for compliance if applicable
3. Update guidelines if pattern emerges repeatedly

**Red Lines (Never Deviate):**
- Security practices (bandit, secret management)
- Version control hygiene (no direct main commits)
- Dependency management through uv
