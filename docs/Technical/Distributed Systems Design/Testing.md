# Testing

There are different types of tests that serve different purposes and are often used collectively in modern systems.

## Functional Testing: Tests that Validate Correctness of a System
These tests are used to verify inputs and outputs of a system.

The difference between these tess is **scope** of the test in terms of components. Usually there are many unit tests, fewer integration tests, and even fewer system tests.

- **Unit Testing** - testing individual sofware units e.g. functions, methods, class, object etc.

- **Integration Testing** - process of testing software units as a group i.e. testing of an entire component or subsystem of a system. The goal is to verify that different software units are interacting as expected. For example software units could function correctly as individual units but incorrectly when they interact.

- **System/End-to-end Test** - process of testing an entire system from start to end, where often the external client application is used as the test entry point. The attempt is to simulate production behavior and verify the system's dependencies, network communication, and interfaces.

## Non-Functional Testing

- **Regression Testing** - testing if recent code or configuration changes have negatively impacted existing system features. It can be functional or non-functional in practicality.

- **Performance Testing** - process of testing the latency, scalibility and reliability of a system or a subsystem.
  - Types of tests:
    - **Load Testing** - test behavior of a system or subsystem under anticipated workloads, discovering bottlenecks or slow performance
    - **Stress Testing** - tests te limit of a system or subsystem uner heavy workloads, identify where the system would break down under the most extreme circumstances
    - **Endurance Testing** - tests the stability of a system or subsystem over a long period, identifying if problems arise over time
  - Key metrics:
    - **CPU Usage** - the amount of CPU used to execute a workload
    - **Memory Usage** - the amount of RAM used during a workload
    - **Disk Usage** - the amount of hard drive or SSD used during a workload
    - **Bandwidth** - the byte per second sent over the network interface during a workload
    - **Response Time** - the time elapsed from a request is sent to when a response is recieved
    - **Throughput** - the number of requests per second that are processed by the system

- **Security Testing** - testing risks, vulnerabilities, and weaknesses in a system or a subsystem. Involves scanning for vulnerabilities and discovering network and system weaknesses. This verifies that the system can protect itself against malicious attacks such as a **denial-of-service attack (DoS)** or a **Distributed Denial-of-Service Attack (DDoS)** where a malicious user attempts to overload a system by sending an overwhelming number of requests. 

- **A/B Testing** - compare varients of the same component and measure performance through randomized experiments. See [Experimentation Notes](../Experimentation/Outline.md)

## Flakiness

A flaky tet is a test that inconsistently passes - i.e. it is non-deterministic. Usually this can be attributed to:

1. **test order** - if tests generate state, they may impact each other, and a test pass may rely on other tests to run in a particular order
2. **race conditions** - threads and processes use synchronization mechanisms (e.g. semaphore or mutexes) where there may be race conditions that cause flakiness.
3. **assertion timing** - asserts or checks are run when tests are in inconsistent states, resulting in flaky tests