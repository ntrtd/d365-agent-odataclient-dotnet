# Implementation Plan: d365-agent-odataclient-dotnet

This document outlines the phased implementation plan for the `d365-agent-odataclient-dotnet` repository. Its purpose is to generate and package the C# OData client library for Dynamics 365, which is consumed by the `d365-agent-mcpserver-dotnet`.

## Overall Goal
To establish a reliable and automated process for generating a type-safe C# OData V4 client from Dynamics 365 metadata and publishing it as a versioned NuGet package.

---

## Phase 1: Initial Client Generation and Packaging

*   **Objectives:**
    *   Set up the .NET project structure for OData client generation.
    *   Successfully generate the C# client code from D365 metadata.
    *   Configure and test NuGet package creation.
    *   Establish a CI/CD pipeline for automating generation and publishing.
*   **Tasks:**
    1.  **Project Setup:**
        *   Create a .NET class library project (e.g., targeting .NET Standard 2.0 or a compatible .NET version for broad usability).
        *   Organize project files and directories.
    2.  **OData Client Generation Tooling:**
        *   **Option A (OData Connected Service):** If using Visual Studio, install and configure the "OData Connected Service" extension. Point it to the D365 OData $metadata endpoint or a local `d365_metadata.xml` file (e.g., included as a linked file or submodule from `d365-agent/asset/d365_metadata.xml`).
        *   **Option B (Command-Line Tooling):** Set up the `Microsoft.OData.Client.Design` NuGet package and configure its T4 templates for code generation. Create scripts to invoke the generation process using the D365 metadata.
    3.  **Initial Code Generation:**
        *   Perform an initial run of the code generation process.
        *   Review the generated C# proxy classes and `DataServiceContext` derivative.
        *   Ensure the generated code compiles successfully.
    4.  **NuGet Package Configuration:**
        *   Edit the `.csproj` file to include all necessary NuGet package metadata:
            *   `PackageId`: (e.g., `D365.Agent.ODataClient.DotNet`)
            *   `Version`: (e.g., `1.0.0`)
            *   `Authors`, `Description`, `RepositoryUrl`, etc.
    5.  **Build and Pack:**
        *   Implement build scripts (`dotnet build`, `dotnet pack`) to compile the library and create the `.nupkg` file.
    6.  **CI/CD Pipeline Setup (e.g., GitHub Actions):**
        *   Create a workflow that:
            *   Checks out the code.
            *   (Optional but recommended) Fetches the latest `d365_metadata.xml` from the `d365-agent` repository (e.g., using `git archive` or by downloading the raw file).
            *   Runs the OData client code generation script/command.
            *   Builds the .NET library.
            *   Packs the NuGet package.
            *   Publishes the NuGet package to a designated feed (e.g., GitHub Packages, Azure Artifacts) upon tagging a new version or merging to the main branch.
*   **Testing:**
    *   Verify that the code generation process runs successfully in the CI pipeline.
    *   Ensure the generated library compiles without errors.
    *   Manually test consuming the generated NuGet package in a sample .NET project (like `d365-agent-mcpserver-dotnet`).
*   **Deliverables:**
    *   A functional C# OData client library for Dynamics 365.
    *   A published NuGet package (initial version).
    *   An automated CI/CD pipeline for client generation and publishing.

---

## Phase 2 & Beyond: Maintenance and Enhancements

*   **Objectives:**
    *   Keep the OData client up-to-date with D365 metadata changes.
    *   Address any issues and improve the generation process.
*   **Tasks:**
    1.  **Metadata Update Strategy:**
        *   Establish a process for regularly updating the source D365 OData $metadata file.
        *   Trigger regeneration and publishing of the client library when metadata changes.
    2.  **Tooling Updates:**
        *   Monitor for updates to the OData Connected Service extension or `Microsoft.OData.Client.Design` package and related .NET tooling.
        *   Adopt new versions as appropriate, testing for compatibility.
    3.  **Customization of Generated Code (If Necessary):**
        *   If the default generated code requires modifications (e.g., adding helper methods, custom attributes to generated classes), explore using T4 template customization (for `Microsoft.OData.Client.Design`) or partial classes.
    4.  **Troubleshooting & Bug Fixes:**
        *   Address any issues encountered during the code generation process or bugs found in the generated client code when used by `d365-agent-mcpserver-dotnet`.
    5.  **Documentation:**
        *   Maintain clear documentation within this repository on:
            *   How to set up the generation environment.
            *   How to regenerate the client.
            *   How to manage dependencies on the D365 metadata source.
            *   Versioning strategy for the NuGet package.
*   **Testing:**
    *   Ongoing validation that new versions of the generated client work correctly with `d365-agent-mcpserver-dotnet`.
    *   Tests to ensure that changes in D365 metadata (new entities, fields, or actions) are correctly reflected in the regenerated client.

This plan ensures that the `d365-agent-odataclient-dotnet` repository effectively provides and maintains the crucial C# OData client library needed for Dynamics 365 interactions by the .NET MCP Server.
