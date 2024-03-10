# Coupling and Cohesion

## Loose Coupling

Different modules, components, and services should have minimal dependency on each other.

All interactions ebtween services is thorugh service interfaces, and implementation details are hidden from other services. Deprecating ro chaning logic can be achieved with only interface changes.

## High Cohesion
The logi, methods, and classes of a single service should functionally be related and support a single purpose. This helps define boundaries between services and makes services easier to maintain.

This allows for:

1. easier maintainence and deployment of services
2. esier testing - a functional change is limited to only relevant services
3. easier to understand code

**Low cohesion** means that 

1. functionality that logically belongs to a single service is spread out
2. unrelated methods and logic are grouped together

==Rule of Thumb==: CRUD operations for a single data model should belong to a single service

