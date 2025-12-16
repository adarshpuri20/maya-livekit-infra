# CLAUDE.md – LiveKit Stack

## Contents

- `docker-compose.yaml` – LiveKit server, Redis, Egress services.
- `livekit.yaml` – LiveKit server configuration (ports, keys, webhook URLs).
- `egress.yaml` – Egress configuration (S3, Chrome flags, LiveKit connection).

## How to run

cd livekit
docker compose up -d
docker compose logs -f livekit
docker compose logs -f egress

text

- LiveKit: `ws://localhost:7880` internally, `wss://livekit.mayacam.link` externally via Nginx / DNS.
- Egress:
  - Loads layout from `/home/egress/layout`.
  - Writes MP4s to S3: `my-live-feed-app-bucket-0`.

## Conventions and safety

- Network:
  - Uses `network_mode: host`; be careful with port conflicts.
- Security:
  - `cap_add: SYS_ADMIN` for egress; required for Chrome sandbox.
  - S3 IAM role is attached to the EC2 host, not configured via static keys.
- Webhooks:
  - `livekit.yaml` webhook URL must match backend path `/webhooks/livekit`.

## How Claude should work in this repo

1. For changes (ports, keys, egress behavior, additional services):
   - Propose edits to YAML files only.
   - Document any required security group or IAM changes explicitly.
2. Always provide:
   - `docker compose` commands to apply changes.
   - Validation steps (“check logs for X”, “verify recording flow”).
3. Avoid introducing new dependencies or containers without clearly explaining their purpose and resource impact.

All YAML changes must go through Git with feature branches and PRs.
