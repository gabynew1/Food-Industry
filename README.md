# Food-Industry

# Food Raw Material Questionnaire Generator

Generate synthetic supplier questionnaires for food industry raw materials in DOCX format, based on the HARIBO supplier questionnaire structure.

## Overview

This Python script creates **Syntetic unique DOCX files**, each one a filled-out _raw material questionnaire_ for a fictional food ingredient (e.g., "Dried Strawberries", "Pea Protein Isolate"). Each document is structured to closely resemble standard information requests from large food manufacturers.
Questions covered include:
- Product specification (composition, process, properties)
- Ingredients and their origins
- Confirmation of EU food law compliance
- Statement on allergens, flavours, colours, GMOs, vegan/vegetarian/palm oil status
- Nutrition declaration and more

Data for each file is generated synthetically and is structurally realistic, but entirely fictional.

## Features

- 50 DOCX forms with plausible supplier/product data
- Each document covers 16+ quality/regulatory topics
- Output is ready for database seeding, workflow simulation, or machine learning experimentation

## Requirements

- Python 3.8 or higher
- python-docx
- faker

Install dependencies:
