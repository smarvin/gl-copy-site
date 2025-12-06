# GitHub Private ↔ GitHab Public Sync Strategy

This project uses a bi-directional sync setup:

- **GitHub public ➝ GitHub private**:
  - A GitHub action triggers a GitHub pipeline when the public `mirror` branch is updated.
  - GitHub private pulls the GitHub public `mirror` branch into its `current` branch using a `sync-from-public` job.

- **GitHub private ➝ GitHub public**:
  - When a user pushes or merges into `current` branch in GitHub private, the `sync-to-public` job mirrors those changes back to GitHub pubic `mirror` branch.
  - To prevent looping or overwriting changes, the job is skipped if the `currrent` was updated by a sync with the public `mirror` branch.

This setup ensures that GitHub public can serve as a public PR interface while GitHub private remains the source of truth.
