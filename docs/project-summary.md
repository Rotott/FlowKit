# Project Summary

This project is a reusable workflow execution framework written in modern C++20. Rather than solving a specific business problem, it provides a generic engine for defining and executing workflows composed of tasks connected by dependencies in a Directed Acyclic Graph (DAG). Applications define their own task implementations, while the framework is responsible for validating the workflow, determining a valid execution order, and coordinating execution.

The project was created as a portfolio piece to consolidate and demonstrate the software engineering knowledge I developed during my Bachelor's degree in Systems Development at Malmö University. It also reflects my interest in software architecture, system design, and workflow orchestration while giving me an opportunity to continue developing and maintaining my C++ skills.

The primary goal is not to build a production-ready workflow engine, but to demonstrate sound software engineering practices through a clean, extensible architecture. The project emphasizes modern C++20, SOLID principles, graph algorithms, API design, and maintainability. It is being developed using Test-Driven Development (TDD) to encourage incremental design, comprehensive unit testing, and confidence when refactoring.

[← Back to README](../README.md)