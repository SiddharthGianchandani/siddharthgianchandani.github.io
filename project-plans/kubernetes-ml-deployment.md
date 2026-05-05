# Multilingual OCR On Kubernetes

## Goal

Build and deploy an OCR service on Kubernetes that converts images into text for
English and major Indian languages: Hindi, Gujarati, Marathi, Sanskrit, Tamil,
and Telugu.

This project should teach practical Docker, Kubernetes, and production
maintenance concepts by turning an ML model into a service that other people can
actually call through an API.

## Portfolio Pitch

A production-style multilingual OCR platform that accepts image uploads, runs
text detection and recognition, returns structured OCR output, and demonstrates
how ML services are containerized, scaled, monitored, and maintained in
corporate environments.

## Core Learning Outcomes

- Build Docker images for Python ML services.
- Understand image layers, dependency management, model artifacts, and container
  startup behavior.
- Deploy services with Kubernetes Deployments, Services, ConfigMaps, Secrets,
  persistent storage, probes, resource limits, and autoscaling.
- Learn operational practices: logging, metrics, health checks, rollouts,
  rollback, incident debugging, versioning, and maintenance.
- Practice API design for a real ML inference service.
- Learn tradeoffs between accuracy, latency, cost, and reliability.

## OCR Engine Strategy

Start with an existing OCR engine before training a custom model.

Primary candidate: PaddleOCR.

- Strong current support for multilingual OCR pipelines.
- Official docs list language options including Hindi, Marathi, Sanskrit, Tamil,
  and Telugu.
- PP-OCRv5 multilingual documentation mentions datasets/models for Devanagari,
  Tamil, Telugu, Sanskrit-related Arabic/Sanskrit grouping, and English.

Baseline/fallback candidate: Tesseract OCR.

- Mature open-source OCR engine.
- Official tessdoc language tables include Gujarati, Hindi, Marathi, Sanskrit,
  Tamil, Telugu, and English language data.
- Useful as a baseline for comparison, even if accuracy varies by language and
  image quality.

Decision for MVP: expose PaddleOCR through an API and keep Tesseract as a
comparison endpoint or later benchmark.

## Language Scope

| Language | Script | Initial Engine Approach |
| --- | --- | --- |
| English | Latin | PaddleOCR and Tesseract baseline |
| Hindi | Devanagari | PaddleOCR primary, Tesseract baseline |
| Gujarati | Gujarati | Validate PaddleOCR support; Tesseract baseline |
| Marathi | Devanagari | PaddleOCR primary, Tesseract baseline |
| Sanskrit | Devanagari | PaddleOCR primary, Tesseract baseline |
| Tamil | Tamil | PaddleOCR primary, Tesseract baseline |
| Telugu | Telugu | PaddleOCR primary, Tesseract baseline |

Note: The user-facing spelling should be `Telugu`.

## MVP Scope

- HTTP API that accepts image uploads.
- Language parameter: `eng`, `hin`, `guj`, `mar`, `san`, `tam`, `tel`, or `auto`.
- OCR response with extracted text, bounding boxes, confidence scores, language,
  processing time, and engine version.
- Docker image for local service execution.
- Kubernetes manifests for local deployment on Kind or Minikube.
- Basic web UI for uploading an image and viewing recognized text.
- Health, readiness, and metrics endpoints.
- README with local Docker and Kubernetes commands.

## API Design

### `POST /ocr`

Input:

- `image`: uploaded file.
- `language`: requested OCR language or `auto`.
- `engine`: optional, initially `paddleocr`.

Output:

```json
{
  "request_id": "ocr_123",
  "language": "hin",
  "engine": "paddleocr",
  "text": "recognized text",
  "blocks": [
    {
      "text": "recognized line",
      "confidence": 0.94,
      "box": [[10, 20], [180, 20], [180, 52], [10, 52]]
    }
  ],
  "processing_ms": 842,
  "model_version": "ppocr-v5"
}
```

### `GET /healthz`

Returns whether the API process is alive.

### `GET /readyz`

Returns whether models are loaded and ready for inference.

### `GET /metrics`

Returns Prometheus-style metrics in a later milestone.

## Suggested Tech Stack

- API: Python, FastAPI, Uvicorn.
- OCR: PaddleOCR primary, Tesseract optional baseline.
- Image processing: Pillow and OpenCV.
- Containerization: Docker.
- Local Kubernetes: Kind first, Minikube later if needed.
- Kubernetes objects: Deployment, Service, Ingress, ConfigMap, Secret,
  HorizontalPodAutoscaler, PersistentVolumeClaim if model caching needs it.
- Observability: structured JSON logs, Prometheus metrics, Grafana dashboard.
- CI/CD: GitHub Actions for tests, Docker build, and manifest validation.

## Repository Structure

```text
multilingual-ocr-k8s/
  app/
    main.py
    schemas.py
    ocr/
      paddle_engine.py
      tesseract_engine.py
      language_map.py
    observability/
      logging.py
      metrics.py
  web/
    index.html
    styles.css
    app.js
  tests/
    test_api.py
    test_language_map.py
  k8s/
    deployment.yaml
    service.yaml
    ingress.yaml
    configmap.yaml
    hpa.yaml
  scripts/
    run-local.ps1
    build-image.ps1
    deploy-kind.ps1
  Dockerfile
  requirements.txt
  README.md
```

## Docker Learning Path

1. Run the FastAPI service locally without Docker.
2. Create a simple Dockerfile.
3. Pin Python dependencies.
4. Add system packages needed for OCR/image processing.
5. Learn build context and `.dockerignore`.
6. Optimize image size with fewer layers and cache-friendly dependency install.
7. Add non-root container user.
8. Compare cold-start behavior when models are downloaded at runtime vs baked
   into the image.

## Kubernetes Learning Path

1. Create local cluster with Kind.
2. Deploy one OCR API pod.
3. Expose it with a Kubernetes Service.
4. Add readiness and liveness probes.
5. Configure environment variables through ConfigMap.
6. Add CPU and memory requests/limits.
7. Scale replicas manually.
8. Add HorizontalPodAutoscaler.
9. Test rolling updates and rollbacks.
10. Add Ingress for browser/API access.
11. Inspect logs, describe pods, exec into containers, and debug failed rollouts.

## Corporate Maintenance Practices

- Version models separately from application code.
- Keep immutable Docker image tags for releases.
- Add health and readiness probes before exposing traffic.
- Log request IDs, language, latency, engine, confidence summary, and errors.
- Track latency percentiles by language and image size.
- Set request size limits to avoid accidental overload.
- Add rate limiting at API gateway or ingress later.
- Use Secrets for private API keys if external storage or monitoring is added.
- Use resource limits because OCR can be CPU and memory heavy.
- Document rollback commands.
- Add runbooks for common failures: model download failure, out-of-memory pod,
  slow inference, bad image input, and crash loop.

## Milestones

1. Local FastAPI OCR endpoint using one language and one sample image.
2. Add language parameter and support English plus one Indic language.
3. Expand to Hindi, Gujarati, Marathi, Sanskrit, Tamil, Telugu, and English.
4. Add simple web upload UI.
5. Containerize with Docker.
6. Deploy to Kind with Deployment and Service.
7. Add probes, ConfigMap, resource limits, and structured logs.
8. Add benchmark script for accuracy and latency by language.
9. Add HPA and load test.
10. Add CI checks and deployment documentation.
11. Write portfolio case study with screenshots, architecture diagram, and
    lessons learned.

## Evaluation Plan

Create a small benchmark set:

- 5 clean printed samples per language.
- 5 noisy phone-photo samples per language.
- 3 mixed-language images.
- 3 low-quality edge cases: blur, skew, low light.

Measure:

- Character error rate where ground truth is available.
- Word error rate for clean documents.
- Average latency.
- P95 latency.
- Failure rate.
- Confidence score distribution.
- Memory usage per pod.

## Risks And Open Questions

- Gujarati support needs early validation in the chosen PaddleOCR version.
- Sanskrit may require careful testing because model support can depend on
  script and training data quality.
- Mixed-language images may need either language detection or multiple OCR passes.
- OCR accuracy will vary heavily by font, scan quality, handwriting, and layout.
- Running multiple models in one pod may increase memory usage.
- GPU support is optional and should not be required for the first version.

## Stretch Features

- Auto language detection.
- PDF input support.
- Batch OCR job queue.
- Async job API for large files.
- Object storage for uploaded files and results.
- Authenticated API keys.
- Admin dashboard for latency, errors, and language usage.
- Canary deployment for new model versions.
- Helm chart.
- Cloud deployment with managed Kubernetes.

## Website Card Copy

Multilingual OCR on Kubernetes is a production-style OCR service for English,
Hindi, Gujarati, Marathi, Sanskrit, Tamil, and Telugu. It turns images into text
through a FastAPI service deployed with Docker, Kubernetes, health checks,
autoscaling, logs, and operational runbooks.

## References To Validate During Build

- PaddleOCR official OCR usage and multilingual language documentation.
- PaddleOCR PP-OCRv5 multilingual documentation.
- Tesseract official tessdoc language data tables.
