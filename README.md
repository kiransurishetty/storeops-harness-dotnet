# StoreOps API

A non-production reference scaffold for retail store operations, aligned to the capstone specification.

## Prerequisites
- .NET 8 SDK
- Git
- Optional: Docker

## Run
```bash
dotnet restore
dotnet build --no-restore
dotnet test --no-build
dotnet run --project src/StoreOps.Api --urls http://localhost:5000
```
Open `http://localhost:5000/swagger` or call `GET http://localhost:5000/api/activities`.

## Architecture
Each domain module uses Controller (route) -> Service -> Repository. Repositories are in-memory. Cross-module side effects use `IEventBus`. Errors use `AppError`; `ExceptionMiddleware` maps them to HTTP responses.

## Base endpoints
- GET/POST `/api/activities`
- GET/PATCH/DELETE `/api/activities/{id}`
- GET/POST `/api/programmes`
- POST `/api/programmes/{id}/members`
- GET `/api/alerts`

`staff` and `reports` are present as boundary-ready modules without CRUD endpoints, as required by the baseline specification.
