---
name: hugo-env-setup
description: Skill for setting up the local development environment for NSoft America Hugo blog.
---

# Hugo Environment Setup Skill — NSoft America

## Role
You are a development environment setup specialist. 
Your goal is to ensure that a new developer (or AI agent) can immediately start blogging by configuring all dependencies correctly.

---

## Core Setup Steps (The "Zero-to-Hero" Workflow)

### 1. Install Hugo Extended (Mandatory)
The PaperMod theme and NSoft's custom SCSS require the **Extended** version of Hugo.
- **Windows**: Use `winget install Hugo.Hugo.Extended --accept-package-agreements --accept-source-agreements --silent`.
- **Mac**: Use `brew install hugo`.
- **Action**: After installation, the user MUST restart their terminal to pick up the updated PATH.

### 2. Initialize Submodules
The theme is stored as a Git Submodule. Without it, the site will fail to build.
- **Action**: Run `git submodule update --init --recursive` in the project root.

### 3. Verify Git Configuration
Ensure the user has a name and email set for commits.
- **Action**: Run `git config user.name` and `git config user.email`. If empty, prompt the user to set them.

---

## Verification & Health Check

After setup, run these commands to verify the environment:
1. `hugo version` -> Must include "extended".
2. `ls themes/PaperMod` -> Should contain the theme files.
3. `git remote -v` -> Should point to NSoft-America-Inc GitHub Org.

---

## Local Preview Guide
Help the user run the local development server to see their changes in real-time.
- **Command**: `hugo server`
- **URL**: `http://localhost:1313`

---

## Automation Script (Optional)
If the user prefers a single command, you can propose creating a `setup.ps1` (or `setup.sh`) that bundles these actions. However, ensure the user understands that terminal restart is required for PATH changes to take effect.
