# Architecture

SafeScan is built as a lightweight front end that expects a serverless AWS pipeline behind it.

## Request flow

1. The user selects an image in the browser.
2. The browser uploads the file to Amazon S3.
3. An S3 event triggers AWS Lambda.
4. Lambda calls AWS Rekognition to analyze PPE compliance.
5. Lambda writes the scan record to DynamoDB.
6. The browser polls API Gateway for the latest results.

## Data contract

The UI expects each scan item to contain at least:

- `timestamp`
- `imageKey`
- `personsDetected`
- `personsCompliant`
- `personsViolating`
- `personsDetail`

Each person record is expected to include:

- `personId`
- `confidence`
- `bbox`
- `detectedPPE`
- `isCompliant`

Each PPE detection item is expected to include:

- `type`
- `bodyPart`
- `confidence`
- `coversBodyPart`
- `bbox`

## Why this structure works well for a portfolio

- The front end is easy to demo without a build step.
- The backend contract is simple enough to explain in interviews.
- The visual design communicates product maturity, not just a proof of concept.
- The repo is small, which makes the implementation easy to review.

## Suggested deployment notes

- Host `index.html` from any static site host.
- Use CORS on the S3 bucket if you are uploading directly from the browser.
- Make sure the results API returns JSON with the same field names the UI already consumes.