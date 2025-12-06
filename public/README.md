### GitHub ↔ GitLab Sync Strategy

This project uses a bi-directional sync setup:

- **GitHub ➝ GitLab**:
  - A GitHub Action triggers a GitLab pipeline when the `mirror` branch is updated.
  - GitLab pulls the GitHub `mirror` branch into its `current` branch via a `sync_github_to_gitlab` job.

- **GitLab ➝ GitHub**:
  - When a user pushes or merges into `current` in GitLab, the `mirror_to_github` job mirrors those changes back to GitHub `mirror`.
  - To prevent looping or overwriting changes, the job is skipped if the push was triggered by the `GitLab CI` user (e.g. after a GitHub sync).

> This setup ensures that GitHub can serve as a public PR interface while GitLab remains the source of truth.
