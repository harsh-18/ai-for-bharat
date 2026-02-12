# Requirements Document

## Introduction

Anveshaka Readiness is a small-dataset readiness index system that processes State/UT hospital beds CSV data from data.gov.in to generate readiness scores and insights. The system provides a serverless-first AWS architecture for data ingestion, validation, scoring, and retrieval with comprehensive provenance tracking and explainability features.

## Glossary

- **Anveshaka_System**: The complete readiness index system including all components
- **Data_Ingestion_Service**: Component responsible for loading CSV data into S3
- **Validation_Service**: Component that validates and normalizes data columns
- **Scoring_Engine**: Component that computes derived metrics and readiness scores
- **API_Gateway**: Component providing REST endpoints for data access
- **Provenance_Tracker**: Component maintaining data lineage and processing history
- **Readiness_Score**: Numerical index (0-100) indicating preparedness level
- **State_UT_Data**: Hospital beds CSV data containing 8 columns and ~39 rows
- **Explainability_Engine**: Component providing top 3 contributors to scores

## Requirements

### Requirement 1: Data Ingestion and Storage

**User Story:** As a data analyst, I want to ingest State/UT hospital beds CSV data into the system, so that I can process and analyze readiness metrics.

#### Acceptance Criteria

1. WHEN CSV data is uploaded, THE Data_Ingestion_Service SHALL store it in S3 with timestamp and version metadata
2. WHEN data ingestion occurs, THE Provenance_Tracker SHALL record the source, timestamp, and processing parameters
3. WHEN invalid CSV format is provided, THE Data_Ingestion_Service SHALL reject the upload and return descriptive error messages
4. WHEN data is stored, THE Data_Ingestion_Service SHALL validate that exactly 8 columns are present
5. WHEN data exceeds 1000 rows, THE Data_Ingestion_Service SHALL reject the upload to maintain small-dataset focus

### Requirement 2: Data Validation and Normalization

**User Story:** As a system administrator, I want data to be validated and normalized, so that downstream processing operates on clean, consistent data.

#### Acceptance Criteria

1. WHEN raw data is processed, THE Validation_Service SHALL check for missing values and data type consistency
2. WHEN column headers are processed, THE Validation_Service SHALL normalize them to standard format
3. WHEN numerical values contain formatting inconsistencies, THE Validation_Service SHALL standardize them
4. WHEN validation fails, THE Validation_Service SHALL log specific validation errors with row and column references
5. WHEN validation succeeds, THE Validation_Service SHALL mark the dataset as ready for scoring

### Requirement 3: Readiness Score Computation

**User Story:** As a policy maker, I want readiness scores computed for each state/UT, so that I can identify areas needing attention.

#### Acceptance Criteria

1. WHEN validated data is available, THE Scoring_Engine SHALL compute readiness scores (0-100) for each state/UT
2. WHEN computing scores, THE Scoring_Engine SHALL use configurable weights for different metrics
3. WHEN scores are calculated, THE Scoring_Engine SHALL derive additional metrics from base columns
4. WHEN scoring completes, THE Explainability_Engine SHALL identify the top 3 contributing factors for each score
5. WHEN weights are updated, THE Scoring_Engine SHALL recompute all affected scores

### Requirement 4: API Endpoints and Data Retrieval

**User Story:** As an application developer, I want REST API endpoints to access readiness data, so that I can build dashboards and reports.

#### Acceptance Criteria

1. WHEN a GET request is made to /scores, THE API_Gateway SHALL return all readiness scores with state/UT identifiers
2. WHEN a GET request is made to /details/{state}, THE API_Gateway SHALL return detailed metrics for the specified state
3. WHEN a GET request is made to /export/csv, THE API_Gateway SHALL return the processed data in CSV format
4. WHEN API requests are made, THE API_Gateway SHALL respond within 3 seconds
5. WHEN invalid state identifiers are requested, THE API_Gateway SHALL return appropriate 404 errors

### Requirement 5: Provenance and Explainability

**User Story:** As a researcher, I want to understand how scores are calculated and track data lineage, so that I can validate and reproduce results.

#### Acceptance Criteria

1. WHEN scores are displayed, THE Explainability_Engine SHALL show the top 3 contributing factors with their weights
2. WHEN data processing occurs, THE Provenance_Tracker SHALL maintain complete audit trail of transformations
3. WHEN provenance is queried, THE Provenance_Tracker SHALL return processing history with timestamps and parameters
4. WHEN score calculations change, THE Explainability_Engine SHALL update contributor rankings accordingly
5. WHEN audit trails are requested, THE Provenance_Tracker SHALL provide machine-readable lineage information

### Requirement 6: Serverless Architecture and Performance

**User Story:** As a system architect, I want a cost-optimized serverless deployment, so that the system scales efficiently for small datasets.

#### Acceptance Criteria

1. WHEN components are deployed, THE Anveshaka_System SHALL use AWS Lambda functions for all processing
2. WHEN API requests are received, THE Anveshaka_System SHALL handle them through API Gateway
3. WHEN data is stored, THE Anveshaka_System SHALL use S3 for persistence with appropriate lifecycle policies
4. WHEN system is idle, THE Anveshaka_System SHALL incur minimal costs through serverless architecture
5. WHEN load increases, THE Anveshaka_System SHALL auto-scale Lambda functions within AWS limits

### Requirement 7: Monitoring and Logging

**User Story:** As a DevOps engineer, I want comprehensive logging and monitoring, so that I can troubleshoot issues and track system performance.

#### Acceptance Criteria

1. WHEN any component processes data, THE Anveshaka_System SHALL log processing steps with timestamps
2. WHEN errors occur, THE Anveshaka_System SHALL capture detailed error information in CloudWatch
3. WHEN API calls are made, THE Anveshaka_System SHALL log request/response metrics
4. WHEN system performance degrades, THE Anveshaka_System SHALL trigger CloudWatch alarms
5. WHEN logs are queried, THE Anveshaka_System SHALL provide searchable, structured log entries

### Requirement 8: Visualization and User Interface

**User Story:** As an end user, I want visualization capabilities to understand readiness data, so that I can make informed decisions.

#### Acceptance Criteria

1. WHEN readiness data is available, THE Anveshaka_System SHALL provide interactive charts and graphs
2. WHEN users view visualizations, THE Anveshaka_System SHALL display scores with color-coded indicators
3. WHEN users interact with charts, THE Anveshaka_System SHALL show detailed tooltips with contributing factors
4. WHEN data is updated, THE Anveshaka_System SHALL refresh visualizations automatically
5. WHEN users export visualizations, THE Anveshaka_System SHALL provide downloadable formats (PNG, PDF)