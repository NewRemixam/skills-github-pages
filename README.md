name: Step 0, Start

# This step triggers after the learner creates a new repository from the template
# This step sets STEP to 1
# This step closes <details id=0> and opens <details id=1>

# This will run every time we create push a commit to `main`
# Reference https://docs.github.com/en/actions/learn-github-actions/events-that-trigger-workflows
on:
  workflow_dispatch:
  push:
    branches:
      - main

# Reference https://docs.github.com/en/actions/security-guides/automatic-token-authentication
permissions:
  # Need `contents: read` to checkout the repository
  # Need `contents: write` to update the step metadata
  # Need `pull-requests: write` to create a pull request
  contents: write
  pull-requests: write

jobs:
  # Get the current step from .github/script/STEP so we can
  # limit running the main job when the learner is on the same step.
  get_current_step:
    name: Check current step number
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - id: get_step
        run: |
          echo "current_step=$(cat ./.github/script/STEP)" >> $GITHUB_OUTPUT
    outputs:
      current_step: ${{ steps.get_step.outputs.current_step }}

  on_start:
    name: On start
    needs: get_current_step

    # We will only run this action when:
    # 1. This repository isn't the template repository
    # 2. The STEP is currently 0
    # Reference https://docs.github.com/en/actions/learn-github-actions/contexts
    # Reference https://docs.github.com/en/actions/learn-github-actions/expressions
    if: >-
      ${{ !github.event.repository.is_template
          && needs.get_current_step.outputs.current_step == 0 }}

    # We'll run Ubuntu for performance instead of Mac or Windows
    runs-on: ubuntu-latest

    steps:
      # We'll need to check out the repository so that we can edit the README
      - name: Checkout
        uses: actions/checkout@v3
        with:
          fetch-depth: 0 # Let's get all the branches

      - name: Create my-pages branch, initial Pages files, and pull request
        run: |
          echo "Make sure we are on step 0"
          if [ "$(cat .github/script/STEP)" != 0 ]
          then
            echo "Current step is not 0"
            exit 0
          fi

          echo "Make a branch"
          BRANCH=my-pages
          git checkout -b $BRANCH

          echo "Create config and homepage files"
          touch _config.yml
          printf "%s\n%s\n%s\n\n" "---" "title: Welcome to my blog" "---" > index.md

          echo "Make a commit"
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add _config.yml index.md
          git commit --message="Create config and homepages files"

          echo "Push"
          git push --set-upstream origin $BRANCH
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      # Update README to close <details id=0> and open <details id=1>
      # and set STEP to '1'
      - name: Update to step 1
        uses: skills/action-update-step@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          from_step: 0
          to_step: 1
          branch_name: my-pageshttps://docs.github.com/en/actions/security-guides/automatic-token-authenticationname: Step 0, Start

# This step triggers after the learner creates a new repository from the template
# This step sets STEP to 1
# This step closes <details id=0> and opens <details id=1>

# This will run every time we create push a commit to `main`
# Reference https://docs.github.com/en/actions/learn-github-actions/events-that-trigger-workflows
on:
  workflow_dispatch:
  push:
    branches:
      - main

# Reference https://docs.github.com/en/actions/security-guides/automatic-token-authentication
permissions:
  # Need `contents: read` to checkout the repository
  # Need `contents: write` to update the step metadata
  # Need `pull-requests: write` to create a pull request
  contents: write
  pull-requests: write

jobs:
  # Get the current step from .github/script/STEP so we can
  # limit running the main job when the learner is on the same step.
  get_current_step:
    name: Check current step number
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - id: get_step
        run: |
          echo "current_step=$(cat ./.github/script/STEP)" >> $GITHUB_OUTPUT
    outputs:
      current_step: ${{ steps.get_step.outputs.current_step }}

  on_start:
    name: On start
    needs: get_current_step

    # We will only run this action when:
    # 1. This repository isn't the template repository
    # 2. The STEP is currently 0
    # Reference https://docs.github.com/en/actions/learn-github-actions/contexts
    # Reference https://docs.github.com/en/actions/learn-github-actions/expressions
    if: >-
      ${{ !github.event.repository.is_template
          && needs.get_current_step.outputs.current_step == 0 }}

    # We'll run Ubuntu for performance instead of Mac or Windows
    runs-on: ubuntu-latest

    steps:
      # We'll need to check out the repository so that we can edit the README
      - name: Checkout
        uses: actions/checkout@v3
        with:
          fetch-depth: 0 # Let's get all the branches

      - name: Create my-pages branch, initial Pages files, and pull request
        run: |
          echo "Make sure we are on step 0"
          if [ "$(cat .github/script/STEP)" != 0 ]
          then
            echo "Current step is not 0"
            exit 0
          fi

          echo "Make a branch"
          BRANCH=my-pages
          git checkout -b $BRANCH

          echo "Create config and homepage files"
          touch _config.yml
          printf "%s\n%s\n%s\n\n" "---" "title: Welcome to my blog" "---" > index.md

          echo "Make a commit"
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add _config.yml index.md
          git commit --message="Create config and homepages files"

          echo "Push"
          git push --set-upstream origin $BRANCH
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      # Update README to close <details id=0> and open <details id=1>
      # and set STEP to '1'
      - name: Update to step 1
        uses: skills/action-update-step@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          from_step: 0
          to_step: 1
          branch_name: my-pages<header>

<!--
  <<< Author notes: Course header >>>
  Include a 1280×640 image, course title in sentence case, and a concise description in emphasis.
  In your repository settings: enable template repository, add your 1280×640 social image, auto delete head branches.
  Add your open source license, GitHub uses MIT license.
-->

# GitHub Pages

_Create a site or blog from your GitHub repositories with GitHub Pages._

</header><header>

<!--
  <<< Author notes: Course header >>>
  Include a 1280×640 image, course title in sentence case, and a concise description in emphasis.
  In your repository settings: enable template repository, add your 1280×640 social image, auto delete head branches.
  Add your open source license, GitHub uses MIT license.
-->

# GitHub Pages

_Create a site or blog from your GitHub repositories with GitHub Pages._

</header>

<!--
  <<< Author notes: Step 1 >>>
  Choose 3-5 steps for your course.
  The first step is always the hardest, so pick something easy!
  Link to docs.github.com for further explanations.
  Encourage users to open new tabs for steps!
-->

## Step 1: Enable GitHub Pages

_Welcome to GitHub Pages and Jekyll :tada:!_

The first step is to enable GitHub Pages on this [repository](https://docs.github.com/en/get-started/quickstart/github-glossary#repository). When you enable GitHub Pages on a repository, GitHub takes the content that's on the main branch and publishes a website based on its contents.

### :keyboard: Activity: Enable GitHub Pages

1. Open a new browser tab, and work on the steps in your second tab while you read the instructions in this tab.
1. Under your repository name, click **Settings**.
1. Click **Pages** in the **Code and automation** section.
1. Ensure "Deploy from a branch" is selected from the **Source** drop-down menu, and then select `main` from the **Branch** drop-down menu.
1. Click the **Save** button.
1. Wait about _one minute_ then refresh this page (the one you're following instructions from). [GitHub Actions](https://docs.github.com/en/actions) will automatically update to the next step.
   > Turning on GitHub Pages creates a deployment of your repository. GitHub Actions may take up to a minute to respond while waiting for the deployment. Future steps will be about 20 seconds; this step is slower.
   > **Note**: In the **Pages** of **Settings**, the **Visit site** button will appear at the top. Click the button to see your GitHub Pages site.

<footer>

<!--
  <<< Author notes: Footer >>>
  Add a link to get support, GitHub status page, code of conduct, license link.
-->

---

Get help: [Post in our discussion board](https://github.com/orgs/skills/discussions/categories/github-pages) &bull; [Review the GitHub status page](https://www.githubstatus.com/)

&copy; 2023 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)

</footer>

<!--
  <<< Author notes: Step 1 >>>
  Choose 3-5 steps for your course.
  The first step is always the hardest, so pick something easy!
  Link to docs.github.com for further explanations.
  Encourage users to open new tabs for steps!
-->

## Step 1: Enable GitHub Pages

_Welcome to GitHub Pages and Jekyll :tada:!_

The first step is to enable GitHub Pages on this [repository](https://docs.github.com/en/get-started/quickstart/github-glossary#repository). When you enable GitHub Pages on a repository, GitHub takes the content that's on the main branch and publishes a website based on its contents.

### :keyboard: Activity: Enable GitHub Pages

1. Open a new browser tab, and work on the steps in your second tab while you read the instructions in this tab.
1. Under your repository name, click **Settings**.
1. Click **Pages** in the **Code and automation** section.
1. Ensure "Deploy from a branch" is selected from the **Source** drop-down menu, and then select `main` from the **Branch** drop-down menu.
1. Click the **Save** button.
1. Wait about _one minute_ then refresh this page (the one you're following instructions from). [GitHub Actions](https://docs.github.com/en/actions) will automatically update to the next step.
   > Turning on GitHub Pages creates a deployment of your repository. GitHub Actions may take up to a minute to respond while waiting for the deployment. Future steps will be about 20 seconds; this step is slower.
   > **Note**: In the **Pages** of **Settings**, the **Visit site** button will appear at the top. Click the button to see your GitHub Pages site.

<footer>

<!--
  <<< Author notes: Footer >>>
  Add a link to get support, GitHub status page, code of conduct, license link.
-->

---

Get help: [Post in our discussion board](https://github.com/orgs/skills/discussions/categories/github-pages) &bull; [Review the GitHub status page](https://www.githubstatus.com/)

&copy; 2023 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)

</footer>
