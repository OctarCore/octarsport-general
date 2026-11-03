# API Contract (v1.0, draft)

Single source of truth for the integration between backend, AI service and frontend.
Any change goes through a Pull Request approved by all three developers.

## Endpoints (backend)

### POST /api/analyses
Upload a video. `multipart/form-data` fields: `video` (file), `sport` (e.g. `volleyball`), `movement` (e.g. `spike`).

Response `202 Accepted`:
```json
{ "analysis_id": "3f2a...", "status": "queued" }
```

### GET /api/analyses/{analysis_id}
Returns the processing state: `queued`, `processing`, `completed`, `failed`.
When `completed`, includes a link to the results. When `failed`, includes `error_message`.

### GET /api/analyses/{analysis_id}/results
Returns the analysis result (format below). Returns `409` if the analysis is not completed yet.

## Result format

```json
{
  "schema_version": "1.0",
  "status": "completed",
  "sport": "volleyball",
  "movement": "spike",
  "video": { "fps": 30, "duration_sec": 5.0 },
  "frames": [
    {
      "frame_index": 0,
      "timestamp_sec": 0.0,
      "pose_detected": true,
      "landmarks": {
        "right_elbow": { "x": 0.31, "y": 0.30, "z": -0.15, "visibility": 0.98 }
      }
    }
  ],
  "overall_score": null,
  "errors": []
}
```

## Rules

- `landmarks` will contain all 33 MediaPipe Pose points (snake_case names, e.g. `left_shoulder`).
- If no person is detected in a frame: `"pose_detected": false`, `"landmarks": null`.
- Coordinates `x`, `y` are normalized to `[0, 1]` relative to the frame size.
- `overall_score` stays `null` until the scoring algorithm exists. Do not invent scores or errors.
- `sport` and `movement` are open string values so new sports can be added without schema changes.
