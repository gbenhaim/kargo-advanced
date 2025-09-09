- Updates from the component teams are submitted using PRs that modifies the development overlay.
  The PR is verified using e2e test (no change to the current state)

- This approach solves a race condition between update images and manifests references, since teams update both using their prs.

requirements:

- we still want to have an argocd application for each component.

resource schema
    - option 1: warehouse for each ring -> promotion is copy content from the origin warehouse to the stage (cluster directory) (warehouse and stage will share the same name. a warehouse is also a stage). copied files are pushed to the main branch. no need to build manifests and push them to a branch which represents the stage. I think commit message will be too generic and won't show which components it changes.there is not real promotion of a freight since each set of clusters consume frights from their own warehouse.

    This approach won't work since when defining the stages, the requestedFreight map contains both the source stage and origin warehouse and those should match.

    - option 2: each cluster is a stage (same as options 1). each cluster directory has component (same as option 1). during promotion the reference to the base is updated to match the commit from the freight, manifests are built and pushed to a branch which represents the cluster. what will happen when patches in the cluster directory are modified? we need something to trigger Kargo for updating the manifest in the cluster's branch. solution: create a warehouse for every cluster that has a git source for the cluster's directory where patches are stored.
