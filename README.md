# AL-Go Per Tenant Extension Template

This template repository can be used for managing Per-tenant Extensions (PTEs) for Business Central.

Please go to https://aka.ms/AL-Go to learn more.

# AL-Go Workflow Flow Diagram

```mermaid
flowchart TD
    DEV([Developer]) --> BRANCH[Create feature branch]
    BRANCH --> COMMIT[Commit AL changes]
    COMMIT --> PR[Open Pull Request\nfeature/* to main]

    PR --> PRH["PullRequestHandler\nPull Request Status Check"]
    PRH --> BUILD1[Build and Compile]
    BUILD1 --> TESTS1[Run Tests]
    TESTS1 --> PASS{Pass?}
    PASS -- Fail --> PRBLOCK[PR blocked from merging]
    PASS -- Pass --> PRREADY[PR ready to merge]

    PRREADY --> MERGE[Merge to main]

    MERGE --> CICD["CICD.yaml\npush to main or release/*"]
    CICD --> BUILD2[Build and Compile]
    BUILD2 --> TESTS2[Run Tests]
    TESTS2 --> VERSIONBUMP[Bump version in app.json\ncommit back to main]
    VERSIONBUMP --> ARTIFACT[Publish build artifact]
    ARTIFACT --> NUGETPROD["Deliver to Production NuGet\ncustomer-x-prod\nvia NuGetContext secret"]

    MERGE --> UAT
    MERGE --> REL

    subgraph MANUAL ["Manual Triggers"]
        UAT["ReleaseToUAT\nworkflow_dispatch"]
        REL["CreateRelease\nworkflow_dispatch"]
    end

    UAT --> PRERELEASE["Create GitHub Pre-Release\nvX.Y.Z-preview"]
    PRERELEASE --> NUGETUAT["Deliver to UAT NuGet\ncustomer-x-uat\nvia NuGetUATContext secret"]

    REL --> RELEASE["Create GitHub Release\nvX.Y.Z"]

    subgraph HOTFIX ["Hotfix Flow"]
        RELBRANCH[Create release/* branch] --> HOTCOMMIT[Commit fix]
        HOTCOMMIT --> CICD
    end

    subgraph NUGET_FEEDS ["Azure DevOps NuGet Feeds"]
        NUGETUAT
        NUGETPROD
    end

    subgraph BRANCH_PROTECTION ["Branch Protection - main"]
        PRBLOCK
        PRREADY
    end

    style NUGET_FEEDS fill:#f0f8ff,stroke:#4a90d9
    style BRANCH_PROTECTION fill:#fff8f0,stroke:#d9844a
    style MANUAL fill:#f0fff0,stroke:#4ad94a
    style HOTFIX fill:#fff0f8,stroke:#d94a90
```

## Contributing

Please read [this](https://github.com/microsoft/AL-Go/blob/main/Scenarios/Contribute.md) description on how to contribute to AL-Go for GitHub.

We do not accept Pull Requests on the template repository directly.
