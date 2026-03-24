# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

IOTICOS God Level (GL) is an IoT platform built from an IoT Bootcamp course. This repository is the **services/infrastructure layer** — it orchestrates Docker containers and provides an automated installer. The actual application code (Nuxt.js frontend + Node.js API) lives in a separate repo (`ioticos_god_level_app`) cloned into the `app/` directory during installation.

## Architecture

Three Docker services orchestrated via Docker Compose:
- **Node.js 14** — Nuxt.js frontend (port 3000) + API backend (port 3001)
- **MongoDB 4.4** — Database (`ioticos_god_level` db), also stores EMQX auth rules and ACLs (`emqxauthrules` collection)
- **EMQX 4.2.3** — MQTT broker (MQTT: 1883, WebSocket: 8083, Dashboard: 18083)

Communication flow: Devices → EMQX (MQTT) → MongoDB for auth/ACL; Frontend → API → MongoDB; EMQX → API via webhooks.

## Docker Compose Files

| File | Purpose | Command |
|------|---------|---------|
| `docker-compose.yml` | Development — Mongo + EMQX only | `docker-compose up -d` |
| `docker_compose_production.yml` | Production — full stack (Node + Mongo + EMQX) | `docker-compose -f docker_compose_production.yml up -d` |
| `docker_node_install.yml` | Run `npm install` in isolated container | `docker-compose -f docker_node_install.yml up` |
| `docker_nuxt_build.yml` | Run `npm run build` (Nuxt) in isolated container | `docker-compose -f docker_nuxt_build.yml up` |

## Installation

The main installer is `install_ioticosgl.sh` — an interactive bash script that installs Docker, clones both repos, generates `.env` files from user input, and starts services.

## Environment Configuration

Two `.env` files are generated during installation (not committed):
- **Root `.env`** — MongoDB credentials, EMQX passwords, timezone, external ports
- **`app/.env`** — API port (3001), app port (3000), MongoDB connection, EMQX API credentials, MQTT host/port, webhook config, SSL settings

## Data Persistence

- MongoDB data: `./mongodata/` directory (volume mount)
- EMQX data/logs: Docker named volumes (`vol-emqx-data`, `vol-emqx-log`)
