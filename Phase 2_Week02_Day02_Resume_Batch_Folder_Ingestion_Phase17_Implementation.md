# Resume RAG Application - Phase 17 Folder Ingestion

## Status

Phase 17 adds local folder-based resume ingestion to the existing backend. Phase 18 and retrieval implementation are not included.

## Goal

Allow Postman to provide a local folder path while the backend discovers PDF resumes and ingests them into MongoDB. Files are processed sequentially in groups of no more than five to control API pressure and make retries predictable.

## Configuration

Add these values to `.env`:

```env
RESUME_IMPORT_ROOT=C:\Users\user\Desktop\HR Resume portal\Resumes (1)\Resumes
MAX_BATCH_FILES=5
```

`RESUME_IMPORT_ROOT` is the allowed folder root. The requested folder must be inside this root. `MAX_BATCH_FILES` is capped by the service at five.

The existing settings remain required:

```env
MONGODB_URI=...
MONGODB_DB_NAME=RAGDB
MISTRAL_API_KEY=...
MISTRAL_EMBED_MODEL=mistral-embed
EMBEDDING_DIMENSION=1024
USE_LLM_PARSER=false
MAX_UPLOAD_SIZE_MB=5
```

Credentials must remain private and must not be committed or pasted into Postman collections or documentation.

## Endpoint

```text
POST http://localhost:3000/v1/resume/ingest-folder
```

### Postman setup

1. Select `POST`.
2. Use the URL above.
3. Select **Body -> raw -> JSON**.
4. Send the folder path using JSON escaping for Windows backslashes:

```json
{
  "folderPath": "C:\\Users\\user\\Desktop\\HR Resume portal\\Resumes (1)\\Resumes"
}
```

The backend scans the requested folder recursively for files with the `.pdf` extension. Word files and other non-PDF files are ignored by the current PDF-only pipeline.

## Processing Flow

For each discovered PDF:

1. Calculate a SHA-256 `sourceHash`.
2. Check MongoDB for an existing document with the same hash.
3. Return `skipped` without calling Mistral when the hash already exists.
4. Extract PDF text.
5. Clean the text.
6. Parse structured resume metadata using the algorithm parser.
7. Generate a Mistral embedding.
8. Insert the document into the `resumes` collection.
9. Preserve the original folder PDF.

Files are processed sequentially. The batch size limits the internal groups to five; files are not processed concurrently.

## Response

A successful request returns HTTP 200 and includes:

- `folderPath`
- `batchSize`
- `batches`
- `discovered`
- `succeeded`
- `skipped`
- `failed`
- `results`

Each successful result includes the filename, status, resume ID, embedding model, and embedding dimension. Skipped results identify already-ingested files. Failed results include a safe error code and message.

The response does not include raw resume text or embedding arrays.

## Error Cases

| Case | HTTP status | Error code |
| --- | ---: | --- |
| Missing folder path | 400 | `FOLDER_PATH_REQUIRED` |
| Folder outside allowed root | 403 | `FOLDER_ACCESS_DENIED` |
| Folder import root missing | 500 | `FOLDER_IMPORT_NOT_CONFIGURED` |
| Folder cannot be read | 400 | `FOLDER_NOT_READABLE` |
| Per-file extraction failure | Per-file result | `RESUME_EXTRACTION_FAILED` |
| Per-file embedding failure | Per-file result | `EMBEDDING_FAILED` |
| Per-file storage failure | Per-file result | `INGESTION_FAILED` |

Partial success is supported. A failed file does not prevent other valid PDFs from being stored.

## MongoDB Verification

The current repository writes to:

```text
Database: RAGDB
Collection: resumes
```

After ingestion, verify that:

- Each newly ingested PDF has one document.
- Duplicate files are reported as skipped.
- `sourceHash` is present.
- `rawText` is not empty.
- `skills` is an array.
- `embedding` is an array.
- `embeddingModel` is `mistral-embed`.
- `embeddingDimension` is `1024`.
- Failed or non-PDF files do not create documents.
- Original PDFs remain in the import folder.

The `.env` value `MONGODB_COLLECTION_NAME=Resume_Storage` is currently not used by the repository; the implementation uses the hardcoded `resumes` collection.

## Logging

The request is logged in:

```text
logs/requests.log
```

The log includes the request ID, endpoint, HTTP status, error code when applicable, and total duration. It does not include raw resume text, vectors, or credentials.

## Validation Completed

- `npm run build` passes.
- Existing Phase 16 tests pass: 1 suite and 9 tests.
- Missing folder path returns HTTP 400.
- The server successfully connected to MongoDB during startup.
- Folder ingestion endpoint returned HTTP 200 during testing.
- Source PDFs are preserved during folder imports.

## Operational Notes

- Test with a small folder containing one to five PDFs before processing the full folder.
- Remove or move temporary test subfolders outside the import root for clean counting.
- Keep `USE_LLM_PARSER=false` because the current LLM parser is a placeholder.
- Rotate any credentials that have been exposed and restart the server after updating `.env`.
