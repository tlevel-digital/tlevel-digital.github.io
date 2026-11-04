# Problem Solving

## Overview

Problem solving is a core skill in digital support and services. Technicians and analysts must be able to break down complex issues, identify patterns, design algorithms, and propose efficient solutions using structured approaches.

---

## Computational Thinking

### 1. Decomposition
Breaking down a complex problem into smaller, more manageable parts.

> Example: Troubleshooting a network issue by isolating whether it’s a device, cable, or router fault.

### 2. Pattern Recognition
Identifying similarities or trends in problems or data sets.

> Example: Spotting that error messages only appear when a specific device connects to the network.

### 3. Abstraction
Removing unnecessary detail to focus on what's important.

> Example: Focusing only on software configurations rather than hardware when resolving an application crash.

### 4. Algorithm Design
Creating a step-by-step procedure to solve a problem or perform a task.

> Example: Writing a flowchart to represent the steps for logging a support ticket and escalating it.

---

## Problem-Solving Strategies

### Top-Down Approach
Start with the overall system and break it down into sub-components.

> Used when: Designing a new helpdesk system from scratch.

### Bottom-Up Approach
Start by solving or understanding low-level components, then integrate them into a system.

> Used when: Testing network cables before examining routers or servers.

### Modularisation
Divide a system into independent modules that can be developed and tested separately.

> Used when: Updating the authentication module of a larger user management system.

---

## Root Cause Analysis (RCA)

A technique used to find the original cause of a problem.

- **5 Whys**: Ask "why" multiple times until you reach the root.
- **Fishbone Diagram (Ishikawa)**: Visual tool categorising potential causes.

> Example: A server crash due to overheating may trace back to blocked airflow caused by poor cable management.

---

## Testing & Evaluation

- **Test Plan**: Structured document outlining tests to validate functionality.
- **Iteration**: Repeating a process until the correct outcome is achieved.
- **Selection**: Using decision points (e.g., `if`, `else`) to choose actions based on conditions.

---

## Flowcharts in Problem Solving

Flowcharts are useful for visualising algorithms and decision-making processes.

**Example Symbol Use:**

- Terminator → Start/End
- Process → Action Step
- Decision → True/False Output

```text
[Start] → [Is device powered on?] → Yes → [Check cable] → ...
                               ↓
                              No
                         [Plug in device]
```
## Further Reading

- [Kolb's Experiential Learning Cycle](supporting-frameworks/kolbs-experiential-learning-cycle.md)
- [Agile vs Waterfall Methodologies](supporting-frameworks/agile-vs-waterfall-methodologies.md)
- [IEEE and ISO Problem Resolution Guidelines](supporting-frameworks/ieee-and-iso-problem-resolution-guidelines.md)
