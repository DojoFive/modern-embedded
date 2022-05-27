---
title: "Managed Environments"
draft: true
summary: "*Exactly the same environment for all builds, fast and verifiable.*"
id: 2
---

*Exactly the same environment for all builds, fast and verifiable.*

Your build environment consists of the compiler, the linker, and additional build scripts and build tools - everything that is required from when you start your build to when you produce a final, tested, releasable binary.

These build environments are managed, traceable, and quickly stood up through container technologies. This has multiple benefits:
- Developer onboarding is fast and painless
- You avoid chasing false bugs due to differing environments
- Upgrades to developer tools is quick and controlled
- Your production CI releases are painless because they use the same environment devs use every day

