# Cold Email Generator

## Overview
The Cold Email Generator is a Python-based tool that automates the process of generating personalized cold emails for business outreach. It utilizes LangChain, Llama 3.1 (via Groq API), and web scraping techniques to extract job descriptions from career pages, match them with relevant portfolio projects, and craft compelling email pitches.

## Features
- **Automated Job Description Extraction**: Scrapes career pages to extract job postings and structures them into JSON format.
- **Skill-Based Portfolio Matching**: Uses a vector database (ChromaDB) to find the most relevant portfolio projects based on job requirements.
- **AI-Powered Email Generation**: Uses Llama 3.1 model to generate well-structured and personalized cold emails.
- **Seamless Integration with LangChain**: Utilizes LangChain components for prompt templating, data processing, and LLM interactions.

## Tech Stack
- **Programming Language**: Python
- **Libraries Used**:
  - `langchain_groq` (for LLM-powered text generation)
  - `langchain_community` (for web scraping and document loading)
  - `langchain_core` (for prompt templating and JSON parsing)
  - `pandas` (for handling portfolio data)
  - `chromadb` (for vector storage and querying)
  - `uuid` (for unique ID generation)

## Installation
### Prerequisites
- Python 3.8+
- Groq API Key
- Required Python libraries (install via pip):

```bash
pip install langchain_groq langchain_community langchain_core pandas chromadb uuid
```

## Usage
### 1. Scrape Job Postings
The script loads a job posting from a given URL and extracts key details like role, experience, required skills, and description.

```python
from langchain_community.document_loaders import WebBaseLoader

loader = WebBaseLoader("https://jobs.nike.com/job/R-33460")
page_data = loader.load().pop().page_content
```

### 2. Extract Job Information
Converts raw scraped text into structured JSON format:

```python
from langchain_core.prompts import PromptTemplate

prompt_extract = PromptTemplate.from_template("""
### SCRAPED TEXT FROM WEBSITE:
{page_data}
### INSTRUCTION:
Extract the job postings and return them as JSON with `role`, `experience`, `skills`, and `description`.
### VALID JSON:
""")
```

### 3. Match Portfolio Projects
Stores and retrieves relevant portfolio links based on job requirements using ChromaDB:

```python
import chromadb
client = chromadb.PersistentClient('vectorstore')
collection = client.get_or_create_collection(name="portfolio")
```

### 4. Generate Cold Email
Uses an AI model to generate a cold email based on the job description and relevant portfolio links:

```python
prompt_email = PromptTemplate.from_template("""
### JOB DESCRIPTION:
{job_description}

### INSTRUCTION:
Write a cold email introducing AtliQ and its expertise.
Use relevant links from {link_list} to showcase portfolio projects.
### EMAIL:
""")
```

### 5. Output Example
```plaintext
Subject: Expert AI/ML Solutions for Nike's Data Science Services and Platform

Dear Hiring Manager,

I came across the job posting for Software Engineer II, AI/ML Platforms at Nike and wanted to introduce ...
```

## Future Improvements
- Enhance job posting extraction with better parsing techniques
- Improve portfolio matching using advanced embeddings
- Support multi-language email generation



