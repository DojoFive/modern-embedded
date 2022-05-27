---
title: "One Source"
summary: "*A single source for all artifacts, each with versioning and traceability.*"
id: 1
---

*A single source for all artifacts, each with versioning and traceability.*

Your source code must be in version control. You should probably use Git, and most teams host their code on cloud-based services.

Your changes are clearly traceable through Git's commit history. Each version of the code has a unique hash.

All of your CI builds are directly mappable to your commit history. The hash of the commits is attached to your build artifacts.

You save valuable artifacts of your build, such as memory maps, test results, debug binaries, to allow quick and easy bisection and debugging.