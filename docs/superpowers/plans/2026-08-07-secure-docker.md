# Secure Docker Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a buildable, non-root production Docker image and prevent sensitive or unnecessary files from entering Git and Docker contexts.

**Architecture:** Use a multi-stage Node.js 22 Alpine image: install and build in intermediate stages, prune to production dependencies, then copy only runtime files into a non-root runner. Keep all exclusion policy in `.dockerignore` and `.gitignore`.

**Tech Stack:** Docker, Node.js 22 Alpine, npm, Next.js 16.3.0

## Global Constraints

- Never inspect secret or credential files.
- Keep all `.env*` files ignored without exceptions.
- Do not enable Next.js standalone output or add dependencies.

---

### Task 1: Secure Docker setup

**Files:**
- Create: `Dockerfile`
- Create: `.dockerignore`
- Modify: `.gitignore`

**Interfaces:**
- Consumes: existing `package-lock.json`, `npm run build`, and `npm start`
- Produces: a Docker image listening on port 3000

- [ ] **Step 1: Verify the Dockerfile is absent and current ignore rules lack the requested Docker safeguards**

Run: `test ! -e Dockerfile && test ! -e .dockerignore`
Expected: exit status 0.

- [ ] **Step 2: Create the minimal multi-stage Dockerfile and ignore rules**

Use Node.js 22 Alpine stages, `npm ci`, `npm run build`, `npm prune --omit=dev`, `COPY --chown=node:node`, and `USER node`. Ignore dependencies, outputs, VCS metadata, logs, `.env*`, keys, Kubernetes/Talos configs, Terraform values/state, and common cloud credential files.

- [ ] **Step 3: Verify formatting, ignore behavior, app build, and Docker build**

Run: `git diff --check && npm run build && docker build -t my-app:test .`
Expected: all commands exit with status 0 and Docker reports a successful image build.
