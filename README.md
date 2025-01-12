## Custom GitHub Actions Template Repository

This repository serves as a template for creating and maintaining custom 
GitHub Actions. It provides a structured approach to developing reusable 
actions that can be shared across multiple repositories.

### Repository Structure
```
├── README.md
├── custom-action-dir/
│   └── action.yml
└── [other-action-dir]/
    └── action.yml
```

You can create multiple action directories, each containing its own action.yml 
file. Each directory represents a separate custom action that can be referenced 
independently.

### Available Actions
#### Example Action: Poetry Installation

The repository includes a sample action in `custom-action-dir/action.yml` to 
demonstrate best practices for action development. This example action handles 
Python Poetry setup with the following capabilities:
- Automated Poetry installation through pip with version control
- Project-level virtualenv configuration for better dependency isolation
- Selective dependency group installation (e.g., testing, development, production)
- Configurable Python and Poetry versions via action inputs

You can use this as a reference when creating your own actions, but feel free 
to delete or modify it for your specific needs. The action structure and 
patterns demonstrated here can be applied to any type of custom action you 
develop.

#### Usage Example
```yaml
uses: your-username/repo-name/custom-action-dir@v1
with:
    python-version: '3.10'  # optional, default: '3.10'
    poetry-version: '2.0.0' # optional, default: '2.0.0'
    dependency-group: 'testing' # optional, default: 'testing'
```

### Development Guidelines
#### Creating New Actions

1. Create a new directory for your action:
    ```bash
    mkdir my-new-action
    cd my-new-action
    ```

2. Create an action.yml file with the required metadata and steps:
    ```yaml
    name: 'Action Name'
    author: 'Your Name'
    description: 'Action description'
    inputs:
    input-name:
        description: 'Input description'
        required: false
        default: 'default-value'
    runs:
    using: "composite"
    steps:
        - name: Step Name
        shell: bash
        run: echo "Your commands here"
    ```


### Versioning and Tags

1. Semantic Versioning:

    - Follow SemVer: MAJOR.MINOR.PATCH
    - Major: Breaking changes
    - Minor: New features, backwards compatible
    - Patch: Bug fixes, backwards compatible

2. Major Version Tags:

    - Create major version tags (v1, v2) that track the latest release:
        ```bash
        # Create and push a specific version
        git tag -a v1.0.0 -m "Release version 1.0.0"
        git push origin v1.0.0

        # Create/update major version tag
        git tag -fa v1 -m "Update v1 tag to v1.0.0"
        git push --force origin v1
        ```
    - Users can reference @v1 to always get the latest v1.x.x release
    - This allows seamless updates for patch and minor versions


3. Version Upgrade Process:

    - When upgrading from v1 to v2:

        1. Create the new version tag:
            ```bash
            git tag -a v2.0.0 -m "Release version 2.0.0"
            git push origin v2.0.0
            ```

        2. Create/update the major version tag:
            ```bash
            git tag -fa v2 -m "Update v2 tag to v2.0.0"
            git push --force origin v2
            ```

        3. Document breaking changes in CHANGELOG.md
        4. Update README with migration guide
        5. Notify users to update their workflows from @v1 to @v2

4. Tag Management Best Practices:
    - Keep major version tags (v1, v2) up to date
    - Never delete or move specific version tags (v1.0.0, v2.0.0)
    - Include pre-release tags if needed: v2.0.0-beta.1
    - Document version compatibility in action.yml


### Release Process

- Update version numbers in documentation and code
- Create a pull request from develop to main
- After approval and merge:
    ```bash
    git checkout main
    git pull
    git tag -a v1.0.0 -m "Release version 1.0.0"
    git push origin v1.0.0
    ```

### Using Custom Actions
To use these actions in your workflows:

    1. Reference the action using the repository path and tag:
        ```yaml
        - uses: your-username/repo-name/action-directory@tag
        ``` 

    2. Specify inputs as needed:
    ```yaml
    - uses: your-username/repo-name/custom-action-dir@v1
      with:
        input-name: value
    ```
