\# Contributing to LoKart



Thank you for contributing to LoKart.



LoKart uses a branch-based development workflow to keep the stable version of the project safe.



\## Branch Structure



\### `main`



`main` contains stable, tested code.



Do not push directly to `main`.



\### `development`



`development` is the main integration branch for ongoing development.



Features should be merged into `development` before they are considered for `main`.



\### Feature Branches



For individual features or larger changes, create a feature branch from `development`.



Example:



```bash

git checkout development

git pull origin development

git checkout -b feature/product-search

```



\## Frontend and Backend



Keep the project organized.



\### Frontend



All frontend work belongs inside:



```text

frontend/

```



\### Backend



All backend work belongs inside:



```text

backend/

```



Do not mix frontend and backend code.



\## Development Workflow



\### 1. Start from development



```bash

git checkout development

git pull origin development

```



\### 2. Create a feature branch



```bash

git checkout -b feature/your-feature-name

```



\### 3. Make your changes



Keep changes focused on the assigned task.



\### 4. Check your changes



```bash

git status

```



Review the files before committing.



\### 5. Commit



```bash

git add .

git commit -m "Describe your change"

```



\### 6. Push your branch



```bash

git push -u origin feature/your-feature-name

```



\### 7. Create a Pull Request



Create a Pull Request from:



`feature/your-feature-name`



into:



`development`



After review and testing, the feature can be merged.



\## Important Rules



\* Do not push directly to `main`.

\* Do not force-push.

\* Do not delete another developer's branch.

\* Do not overwrite another developer's work.

\* Do not commit secrets or `.env` files.

\* Keep frontend code in `frontend/`.

\* Keep backend code in `backend/`.

\* Ask before making major architectural changes.



\## AI-Assisted Development



AI coding tools may be used to help develop LoKart.



AI-generated changes must follow the same repository rules as human-written code.



Always review AI-generated changes before committing them.



