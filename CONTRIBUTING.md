# Contributing

## Overview
Thank you for contributing to LLM Arena.

This repository contains:
- `llm-arena-frontend` for the React frontend
- `llm-arena-backend` for the Django backend
- the root Docker Compose setup for running the full stack together

## Before You Start
- read the root [README.md](/Users/itonkdong/Work/Fax/INSOK/llm-arena/README.md)
- read the app-specific READMEs inside `llm-arena-frontend` and `llm-arena-backend`
- use `.env.example` files as templates
- never commit secrets or real API keys

## Development Flow
- make focused changes
- keep behavior consistent with the current product flow
- prefer small, reviewable pull requests
- update documentation when setup or behavior changes

## Docker And Environment
- use the root `docker-compose.yml` when working on the full stack
- use the per-app compose files when working on one app in isolation
- keep root `.env` for compose-level overrides only
- keep app-specific secrets and settings inside each app folder as intended

## Code Guidelines
- preserve the current project structure
- follow existing naming and organization patterns
- avoid unrelated refactors in the same change
- keep admin/config changes intentional and easy to review

## Pull Requests
- explain what changed and why
- mention any setup, env, or migration impact
- include screenshots for frontend or admin UI changes when useful
- call out any known gaps or follow-up work

## Security
- do not commit `.env` files
- rotate any leaked keys immediately
- double-check Docker build contexts and ignore files before publishing images
