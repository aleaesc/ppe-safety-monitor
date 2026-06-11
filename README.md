# SafeScan

SafeScan is a portfolio-ready PPE compliance dashboard for construction-site photos. It uploads an image to S3, reads analysis results from API Gateway, and visualizes per-person PPE compliance with bounding boxes, scan history, and summary stats.

## Overview

This project is intentionally presented as a single-file front end so it is easy to demo, host, and review. The UI focuses on clarity, fast feedback, and a polished glassmorphism presentation that makes the portfolio itself feel like a product page.

## Features

- Upload flow for JPG and PNG images
- Live dashboard metrics for total scans, compliant people, and violations
- Per-scan history with annotated previews and modal drill-downs
- PPE detail table for helmet, face cover, and hand cover status
- Client-side controls for required PPE, confidence threshold, and strict mode
- Clear-history action for resetting the local dashboard view
- AWS-inspired visual system with service logos and status feedback

## Tech Stack

- HTML5 and CSS3
- Vanilla JavaScript
- AWS S3 for image storage
- AWS Lambda for backend processing
- AWS Rekognition for PPE analysis
- Amazon DynamoDB for results persistence
- Amazon API Gateway for the results API

## Repository Layout

- `index.html` - the full application, styles, and client-side logic
- `README.md` - project overview, setup, and deployment notes
- `docs/architecture.md` - backend contract and system flow
- `logos/` - AWS service icons used in the trust bar

## How It Works

1. Choose an image from the upload area or drag and drop it onto the page.
2. The browser uploads the file directly to the configured S3 bucket.
3. A backend workflow analyzes the image and stores PPE results.
4. The UI polls the configured API endpoint and renders the latest results.

## Local Setup

Open `index.html` directly in a browser, or serve the folder locally if you want a cleaner development workflow.

```bash
python -m http.server 8080
```

Then visit `http://localhost:8080`.

## Deploying to Vercel

This repo is ready for a static Vercel deployment with no build step.

1. Push the repository to GitHub.
2. Import the repo into Vercel.
3. Keep the framework preset as `Other` or `Static`.
4. Leave the build command empty.
5. Set the output directory to the repository root, or let Vercel detect it automatically.

The included `vercel.json` keeps the app on `index.html`, which is enough for this single-page portfolio demo.

## Configuration

Open `index.html` and update the constants in the `<script>` block if your AWS resources differ from the ones in the repo:

- `API_URL` for the results endpoint
- `BUCKET` for the S3 upload bucket
- `REGION` for the AWS region

Use the Settings panel in the app to adjust:

- Required PPE items
- Confidence threshold
- Strict mode

## Backend Expectations

The front end assumes a backend that:

- Accepts direct S3 uploads from the browser
- Stores scan results in DynamoDB
- Exposes the latest results through API Gateway
- Returns a JSON payload with image metadata and per-person PPE detections

If you want the repo to stay strictly front-end only, the app can still be used as a polished demo shell by pointing `API_URL` at mock data.

## Portfolio Notes

- The dashboard refreshes on a timer so it can pick up newly processed scans.
- The Clear History button only resets the displayed client-side results. It does not delete DynamoDB records.
- If public image reads are disabled on S3, the scan detail image preview will fall back to an error state.
- The UI is designed to read well in a GitHub portfolio context even before backend wiring is complete.

## Author

Alea Escala
