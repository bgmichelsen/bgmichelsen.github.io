---
title: "GENERAL PROJECT GUIDELINES"
layout: default
---

# General Project Guidelines

These are the guidelines for starting any project, regardless of discipline. Whenever first starting a personal project, these guidelines should be followed to ensure a streamlined process.

## Design

All projects should start with a design. This can be a pen and paper sketch, a digital diagram, or a whiteboard diagram. Whatever the method, a project must be designed before work is started.

This ensures that the idea is workshopped before money and materials are invested. It also gives a plan of action when creating the prototype.

## Research

Additionally, research should be conducted before starting the project. Research should include:

- Whether or not someone has already accomplished the idea.
- The physics/methods involved in the project.
- Any existing components/software libraries needed to complete the project.

## File Structure

For every project, the following file structure shall be used:

root/

    electrical/

        bom/

        grbr/

        pdf/

    mechanical/

        bom/

        stl/

        pdf/

    software/

        app/

            inc/

            src/

        lib/

            inc/

            src/

        test/

NOTE: Depending on the project, some folders may not be necessary. For example, if a project consists of no mechanical components, that folder and its subfolders can be excluded from the project.

For software libraries/components, the following structure will be used:

root/

    public/

    src/

    test/

### Some Notes on the Folders

The "/grbr" folder contains PCB Gerber files for manufacturing.

The "/bom" folder contains the bill of materials for that discipline of the project (mechanical/electrical).

The "/test" folder contains the unit tests for the software.

## Notes

Always keep notes for a project. This includes updated diagrams for the project, supporting documentation, and test results and data.
