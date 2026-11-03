# OctarSport

Web platform for automatic sports technique analysis from video using computer vision.
First supported sport: volleyball.

## Repositories

| Repository | Description | Stack |
|---|---|---|
| [octarsport-general](https://github.com/OctarCore/octarsport-general) | Documentation, API contract, Docker Compose | Docs, Docker |
| [octarsport-backend](https://github.com/OctarCore/octarsport-backend) | REST API, queue, database | ASP.NET Core, PostgreSQL |
| [octarsport-ai](https://github.com/OctarCore/octarsport-ai) | Pose estimation and analysis | Python, OpenCV, MediaPipe |
| [octarsport-frontend](https://github.com/OctarCore/octarsport-frontend) | Web client | React, TypeScript |

## Data flow

1. User uploads a video in the web client.
2. Backend stores the file and creates an analysis task.
3. Backend sends the video path to the AI service.
4. AI service returns structured JSON (landmarks, errors, score).
5. Backend saves the result and exposes it via API.
6. Frontend shows the video with skeleton overlay and recommendations.

## Documentation

- [API contract](docs/api-contract.md)

## Local setup

1. Clone all four repositories into one folder.
2. Copy `.env.example` to `.env` and fill in the values.
3. Run `docker compose up -d`.

## Workflow

- `main` is protected, changes go through Pull Requests.
- Branch naming: `feature/<short-name>`, `fix/<short-name>`.
- Commit style: `feat(backend): add video upload endpoint`.
- Changes to the API contract require approval from all three developers.
