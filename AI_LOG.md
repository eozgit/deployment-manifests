# 🤖 Project Log: Architectural Decisions (Assisted by Gemini)

This document serves as a high-level log summarizing key architectural and configuration decisions made with the assistance of an AI model (Gemini).

## 1. Core Repository Purpose

*   **Decision:** The repository was defined as the centralized Source of Truth for all deployment manifests, separating configuration from application source code.
*   **Rationale:** To ensure repeatable, verifiable, and version-controlled infrastructure deployment across multiple environments.

## 2. Port Assignment Protocol (MPAP)

*   **Initial Approach:** Considered a complex hashing algorithm for deterministic port assignment (e.g., `Sum of Codes % 10000`).
*   **Final Decision (MPAP):** Switched to a Structured Manual Assignment Protocol (MPAP), recognizing that for a personal portfolio, aesthetic cleanliness and ease of recall outweigh the statistical need for complexity.

## 3. Docker Structure Best Practices

*   **Decision:** Enforce separate, isolated build contexts for frontend and backend services.
*   **Rationale:** To maintain strict separation of concerns, improve build performance by limiting the files sent to the Docker daemon, and adhere to industry-standard security practices.
*   **Refinement (Source Code Acquisition):** For projects whose source code resides in external Git repositories and needs to be fetched during the build, `git clone` is performed directly within the Dockerfile. This also includes correctly identifying and referencing nested project files (e.g., `api/api.csproj`) and their resulting output assemblies (e.g., `api.dll`) in subsequent build steps and the `ENTRYPOINT`.

### MPAP Rules:

*   **Port Range:** Clean `42xxx` block used for all services.
*   **Project Base Port:** Set at `XX060` (e.g., `42060`, `42160`) to enforce a clean look while avoiding common, risky port endings like `80`, `90`, and `00`.
*   **Service Increments:** Services within a project increment backwards by `10` (e.g., `42060`, `42050`).

<h2>3. Docker Structure Best Practices</h2>

*   **Decision:** Enforce separate, isolated build contexts for frontend and backend services.
*   **Rationale:** To maintain strict separation of concerns, improve build performance by limiting the files sent to the Docker daemon, and adhere to industry-standard security practices.
