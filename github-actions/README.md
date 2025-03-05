# Theory

## resources

- github action instances currently provides (for plan): 4-vCPU, 16GB RAM, a cache size limit of 10GB (older cache entries are
  pruned automatically)

## file structure

- location: put github-action workflow file at `.github/workflows` as `.yml` file
- github action consist of followng components
  - event triggers (`on:`)
    - defines the events that will trigger the workflow to run
    - `push`, `pull_request`, `workflow_dispatch`, `schedule`
  - jobs (`jobs:`)
    - defines the actual tasks and actions to be executed when the workflow is triggered
    - each job is a set of steps than run on a specified runner (e.g. Ubuntu, Windows, etc.)
    - jobs can run in parallel or sequentially

# Examples

### action 1

- action is defined in `.github/workflows/script1.yml`, task of which is to execute `script1.py` file in the same directory
- user can manually execute action through following process
  - open this repository on github
  - set .env variable `API_TOKEN_1` at `settings -> security -> secrets and variables -> actions`
  - click on 'Actions' tab, click on 'select workflow'
  - select 'Execute Script#1' which is the name of workflow define under `name` variable in `script1.yml` file
  - click 'Run Workflow', in few seconds you will script execution details
