Technical Specification for Developing an API for Vehicle Management
1. Introduction

The purpose of this project is to develop a RESTful API for managing rides.
The API must provide functionality for creating, retrieving, and updating ride information.

2. General Requirements

The API must support version 1.0.0.

All API requests must use Basic Authentication.

All API responses must be in JSON format.

3. Application Logic
3.1 Updated Endpoints and Their Functionality

In this version of the API, a Basic Authentication mechanism must be added to the endpoints. This includes:

Checking the Authorization header in incoming requests.

Validating user credentials (username and password).
Admin credentials are set as 'admin' and 'qwerty' and stored in environment variables.

Handling failed authorization attempts (e.g., returning 401 Unauthorized).

Endpoints requiring Basic Authentication:

GET /api/drivers

GET /api/drivers/{id}

POST /api/drivers

PUT /api/drivers/{id}

DELETE /api/drivers/{id}

Additionally, for DELETE /api/drivers/{id}:

If a driver’s status is 'on-order' (meaning the driver currently has an active order), the driver cannot be deleted.

3.2 New Endpoints and Their Functionality

This section describes the new endpoints related to ride management.
These endpoints allow administrators to effectively handle the ordering and tracking of rides.

POST /api/rides — Create a Ride

The administrator can create a new ride for a client by specifying client, driver, and start/end address information.

When a ride is created, the system automatically assigns a driver (whose status becomes 'on-ride') and a vehicle.

A ride cannot be assigned to a driver whose status is 'on-ride' (already on a trip) or 'offline' (not active in the system).

Basic Authentication is required.

GET /api/rides — Get All Rides

The administrator can view a list of all active and completed rides.

This allows for tracking ride history and statuses.

Basic Authentication is required.

GET /api/rides/{id} — Get Ride by ID

The administrator can retrieve detailed information about a specific ride using its unique ID.

The response includes client data, driver info, cost, and ride status.

Basic Authentication is required.

4. Project Description

This version introduces major improvements aimed at enhancing functionality, performance, and usability.
Below are the key updates and their benefits compared to the previous version.

Replacement of Manual Validation with express-validator

In the previous version, input validation was handled manually, which increased the risk of errors and made maintenance more difficult.
In this version, the express-validator library is used to automate validation.
For error formatting and response handling, the middleware inputValidationResultMiddleware is used.
This standardizes error handling and improves client communication.

Advantages:

Simplified and cleaner code.

Increased reliability of data validation.

Better user experience with clear and structured error messages.

Introduction of the Repository Layer

A new repository layer isolates database operations from route logic.
Routes now focus only on handling requests and responses, while database interaction logic resides in separate modules.

Advantages:

Cleaner and more maintainable code structure.

Easier testing and modification of database logic.

Increased flexibility and scalability of the application.

Use of Handlers for Request Processing

Each route now includes dedicated handlers — functions responsible for specific types of requests (GET, POST, PUT, etc.).

Advantages:

Clear separation between validation and business logic.

Easier testing of individual logic units.

Simplified expansion — new handlers can be added without modifying existing code.

Implementation of Basic Authentication

Basic Authentication has been added for verifying admin credentials, ensuring an additional layer of security and simplifying authentication logic.

Advantages:

Simplified authentication process.

Enhanced security.

Easier integration with external systems or services.

Improved access control management.

Added Tests for the Ride Management Module

With the introduction of the ride management feature, new tests have been added to verify its functionality.
Common request patterns have been extracted into utility functions to reduce code duplication and improve maintainability.

Advantages:

Increased reliability and stability.

Easier debugging and error detection.

Confidence that new features don’t break existing functionality.

Easier test maintenance and scalability.

Extraction of Route URLs into Constants

All route URLs have been moved into constants for better code readability and easier maintenance.
Now, URL changes can be made in a single location.

Advantages:

Simplified URL management.

Reduced chance of mistakes when updating routes.

Improved code structure and consistency.

Pre-Deletion and Pre-Update Entity Checks

Before performing DELETE or UPDATE operations, the system now executes a GET request by ID to ensure the entity exists.
This prevents redundant repository-level checks and improves overall data handling.

Advantages:

More reliable delete and update operations.

Simplified repository logic.

Improved error handling and user feedback for non-existent entities.

5. Conclusion

This technical specification describes the requirements for updating and improving the Ride Management API.
It highlights key enhancements introduced in this version, including:

Automated input validation using express-validator

A new repository layer

Implementation of Basic Authentication

Additional testing for the new ride management module

These improvements aim to increase the reliability, security, and usability of the application, providing a strong foundation for the further development and expansion of the vehicle management system.
