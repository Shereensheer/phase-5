# Advanced Cloud Deployment Documentation

## Overview
This document describes the Advanced Cloud Deployment phase of the Todo Chatbot system. This phase implements a cloud-native, event-driven architecture using Dapr building blocks, Kubernetes deployment, and asynchronous workflows to achieve scalability and reliability.

## Architecture

### Event-Driven Design
The system implements an event-driven architecture where services communicate through events published to a central event stream. Each service operates independently and reacts to events relevant to its domain.

### Service Components
- **Task Service**: Manages task lifecycle (create, update, delete, complete)
- **Reminder Service**: Schedules and manages reminder delivery
- **Recurring Task Engine**: Generates next occurrence of recurring tasks
- **Notification Service**: Delivers notifications to users
- **Audit Service**: Maintains immutable log of all system activities

### Dapr Integration
All services use Dapr building blocks for:
- Service-to-service invocation
- Event pub/sub
- State management
- Secret management
- Distributed tracing

## Deployment

### Kubernetes Manifests
The system is deployed using Kubernetes manifests with Dapr sidecars configured for each service. Helm charts provide environment-specific configuration.

### Environment Parity
Deployment configurations ensure parity between local (Minikube) and cloud environments.

## Configuration

### Environment Variables
- `SERVICE_NAME`: Name of the service
- `NODE_ENV`: Environment (development, staging, production)
- `DATABASE_URL`: Connection string for the database
- `EVENT_BUS_URL`: Connection string for the event bus

### Dapr Components
- `pubsub`: Event streaming (Kafka-compatible)
- `statestore`: State management (Redis)
- `secretstore`: Secret management (Kubernetes secrets)

## API Endpoints

### Task Service
- `POST /tasks`: Create a new task
- `GET /tasks`: Get all tasks for a user
- `PUT /tasks/{id}`: Update a task
- `DELETE /tasks/{id}`: Delete a task
- `POST /tasks/{id}/complete`: Mark a task as completed

### Reminder Service
- `GET /reminders`: Get all reminders for a user
- `POST /reminders/schedule`: Schedule a new reminder

### Notification Service
- `POST /notifications/send`: Send a notification
- `GET /notifications/{userId}`: Get notifications for a user

## Event Schema

### Task Events
- `TaskCreated`: Emitted when a task is created
- `TaskUpdated`: Emitted when a task is updated
- `TaskCompleted`: Emitted when a task is completed
- `TaskDeleted`: Emitted when a task is deleted

### Reminder Events
- `ReminderScheduled`: Emitted when a reminder is scheduled
- `ReminderDelivered`: Emitted when a reminder is delivered
- `ReminderFailed`: Emitted when a reminder delivery fails

### Notification Events
- `NotificationSent`: Emitted when a notification is sent
- `NotificationDelivered`: Emitted when a notification is delivered
- `NotificationFailed`: Emitted when a notification fails

## Monitoring and Observability

### Health Checks
Each service provides health check endpoints:
- `/health`: General health status
- `/ready`: Readiness for traffic
- `/live`: Liveness probe

### Metrics
Services expose metrics for:
- Request rates and latencies
- Error rates
- Active connections
- Task operations
- Event processing

### Tracing
Distributed tracing is implemented using Dapr's tracing capabilities with correlation IDs for request tracking across services.

## Security

### Authentication
Authentication is handled through the frontend which passes user context to backend services.

### Authorization
Each service validates user permissions for requested operations.

### Secrets Management
Secrets are managed through Dapr's secret store component and Kubernetes secrets.

## Error Handling

### Circuit Breaker
Circuit breaker pattern is implemented for service invocations to prevent cascading failures.

### Retry Logic
Retry mechanisms with exponential backoff are implemented for event publishing and service invocations.

### Graceful Degradation
Services implement graceful degradation when dependencies are unavailable.

## Development

### Local Development
Use Minikube for local development with the same configuration as production.

### Testing
- Unit tests for individual components
- Integration tests for service interactions
- Contract tests for API compatibility

### Deployment
Use the provided Helm charts for deployment to Kubernetes clusters.