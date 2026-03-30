# Tech Stack

## Overview
Flatnotes is a self-hosted, database-less note-taking web application built with a Python/FastAPI backend and Vue.js 3 frontend, deployed via Docker.

## Languages
Python 3.11 (backend), JavaScript ES modules (frontend)

The backend leverages Python's async capabilities with FastAPI. The frontend uses plain JavaScript with Vue.js 3's Composition API — no TypeScript, keeping the build simple and accessible.

## Framework
**Backend**: FastAPI with Uvicorn (ASGI server)
**Frontend**: Vue.js 3 + Vite (build tool) + PrimeVue 3 (UI components) + Tailwind CSS 3 (styling) + Vue Router 4

FastAPI provides automatic OpenAPI docs, async request handling, and Pydantic validation. Vue.js 3 with Vite offers fast HMR and a modern development experience. PrimeVue provides pre-built accessible components. Tailwind CSS handles utility-first styling.

## Authentication
Custom built-in authentication system with multiple modes:
- No authentication
- Read-only (public read, authenticated write)
- Username/password
- 2FA via pyotp (TOTP)

Self-contained — no external auth providers needed, fitting the self-hosted philosophy.

## Infrastructure & Deployment
Docker with multi-stage build:
- Stage 1: Node.js — builds the Vue.js frontend
- Stage 2: Python — serves the app via Uvicorn

Self-hosted deployment model. No cloud provider dependency. Users pull the Docker image and run it with volume mounts for note storage.

## Package Manager
- **Frontend**: npm
- **Backend**: Pipenv

## Decision Relationships
- The database-less design (flat markdown files + Whoosh search index) eliminates traditional ORM/database decisions
- Docker multi-stage build ties the frontend build (npm/Vite) and backend runtime (Pipenv/Uvicorn) into a single deployable artifact
- Self-hosted model means auth must be built-in rather than delegated to a cloud provider
