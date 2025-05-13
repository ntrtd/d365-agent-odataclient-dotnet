# Dynamics 365 AI Agent - D365 OData Client (.NET) (d365-agent-odataclient-dotnet)

This repository is responsible for generating a type-safe C# OData V4 client library for interacting with Microsoft Dynamics 365. The generated client is a crucial dependency for the `d365-agent-mcpserver-dotnet` project.

## Overview

The primary purpose of this repository is to house the tooling, configuration, and process for generating C# proxy classes and a `DataServiceContext` derivative from the Dynamics 365 OData $metadata. This enables strongly-typed access to D365 entities and operations from .NET applications.

## Key Features & Responsibilities

*   **OData Client Generation:** Contains the setup and scripts/tooling configuration (e.g., using OData Connected Service or `Microsoft.OData.Client.Design`) to generate the C# client code.
*   **Metadata Consumption:** Uses a Dynamics 365 OData $metadata XML file (typically sourced from the main `d365-agent/asset/d365_metadata.xml` or directly from a D365 instance) as input for the generation process.
*   **NuGet Packaging:** The generated C# client library is packaged as a NuGet package for easy consumption by other .NET projects, primarily the `d365-agent-mcpserver-dotnet`.
*   **Version Control:** Manages different versions of the generated client as the D365 metadata evolves.
*   **CI/CD Automation:** Aims to automate the client generation and NuGet packaging process.

## Technology Stack

*   **C# / .NET**
*   **OData Connected Service** (Visual Studio Extension) or **`Microsoft.OData.Client.Design`** (T4-based code generation).
*   Build tools (.NET SDK, MSBuild).
*   NuGet packaging tools.

## Interaction Flow

1.  Obtain the OData $metadata XML from the target Dynamics 365 environment.
2.  Use the OData client generation tools (OData Connected Service or T4 templates with `Microsoft.OData.Client.Design`) within this project to process the metadata.
3.  The tools generate C# source code files representing D365 entities, enums, actions, functions, and a `DataServiceContext` subclass.
4.  This project compiles the generated code into a .NET library.
5.  The library is packaged into a NuGet package (e.g., `D365.Agent.ODataClient`).
6.  The `d365-agent-mcpserver-dotnet` project consumes this NuGet package to interact with Dynamics 365 in a type-safe manner.

## Getting Started

(Details to be added - typically involves setting up .NET development environment, ensuring OData Connected Service is available or T4 tooling is configured, and running the generation script/process).
Refer to the `implementation.md` in this repository for a detailed development plan.

## Contribution

(Details on contribution guidelines if applicable, especially around updating metadata and regenerating the client).
