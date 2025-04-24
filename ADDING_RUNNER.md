# Adding a runner in GitLab project

This document describes how you can add a RISC-V GitLab runner in your project.

## Settings in the GitLab project

- Navigate to the GitLab project
- In the left menu, hover the cursor on the `Settings`, then click on the `CI/CD` from the menu shown
- Expand `Runners` and click `New project runner`
- On `New project runner` page,
    
    - Add suitable `Tags`
    - Add a suitable `Runner description`
    - Add a `Maximum job timeout`

- On next page,

    - Choose the `Operating Systems` as `Linux`
    - Copy the token from the `Step 1`
    - Go to URL `<Some Cloud-V URL>`
    - Enter the token there and click `Get Access`

This will register the GitLab runner for the project for 30 days