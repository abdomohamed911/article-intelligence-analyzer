# Article Intelligence Analyzer

An NLP-powered content analysis engine that evaluates news articles, blog posts, and long-form text for sentiment, meaning, and linguistic quality. Built with Express and Node.js, it integrates with external natural language processing APIs to deliver structured, actionable content intelligence.

## Overview

Article Intelligence Analyzer transforms unstructured text into quantitative and qualitative insights. Given a URL or raw text input, the system extracts the article content, analyzes it through multiple NLP dimensions, and returns a comprehensive report covering sentiment polarity, subjectivity, readability, tone, and key phrase extraction.

The tool serves content strategists, editors, researchers, and developers who need to understand the linguistic properties of text at scale -- whether for editorial review, competitive analysis, audience research, or content pipeline automation.

## Features

- **URL and Text Input** -- Accepts both raw text and article URLs, automatically extracting and cleaning content from web sources
- **Sentiment Analysis** -- Determines overall sentiment polarity (positive, negative, neutral) with a confidence score and magnitude rating
- **Subjectivity Scoring** -- Measures the degree to which content is opinion-based versus factual, useful for editorial and research workflows
- **Readability Assessment** -- Calculates multiple readability indices (Flesch-Kincaid, Gunning Fog, SMOG) to evaluate text accessibility for target audiences
- **Tone Detection** -- Identifies the prevailing tone of the content (formal, informal, analytical, persuasive, conversational) for brand consistency checks
- **Key Phrase Extraction** -- Surfaces the most significant terms and phrases in the text, ranked by relevance and frequency
- **Language Detection** -- Automatically identifies the language of the input text and routes it through the appropriate analysis pipeline
- **Batch Processing** -- Supports analyzing multiple articles in a single request for bulk content auditing
- **Structured API** -- Clean RESTful interface with consistent JSON responses, pagination for batch results, and error handling
- **Caching Layer** -- Built-in response caching to avoid redundant API calls for previously analyzed content

## Tech Stack

| Category | Technology |
|---|---|
| Runtime | Node.js |
| Framework | Express |
| Language | JavaScript |
| NLP Processing | External NLP APIs (MeaningCloud, Aylien, or configurable provider) |
| URL Extraction | Content parsing library |
| Caching | In-memory cache with TTL |
| Testing | Jest, Supertest |

## Architecture

```
article-intelligence-analyzer/
  src/
    server/             # Express application setup and middleware
    routes/             # API route definitions
    controllers/        # Request handlers and orchestration
    services/
      analyzer.js       # Core analysis orchestration
      sentiment.js      # Sentiment and subjectivity analysis
      readability.js    # Readability metric calculations
      tone.js           # Tone detection logic
      keywords.js       # Key phrase extraction
      extractor.js      # URL content extraction and cleaning
    middleware/         # Validation, error handling, rate limiting
    utils/              # Response formatting, caching, helpers
    config/             # API keys, provider configuration, defaults
  tests/                # Unit and integration test suites
  .env.example          # Environment variable template
```

**Analysis Pipeline:**

1. The client submits a URL or text payload to the analysis endpoint
2. If a URL is provided, the content extractor fetches the page, removes HTML noise, and extracts the core article text
3. The text is validated for minimum length and language compatibility
4. The analyzer service dispatches the text to configured NLP providers for parallel analysis across all dimensions
5. Results are aggregated, normalized, and structured into a unified response object
6. The response is cached with a TTL based on the content hash for subsequent requests
7. The structured report is returned to the client

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/analyze` | Analyze a single article (URL or text) |
| POST | `/api/analyze/batch` | Analyze multiple articles in a single request |
| GET | `/api/analyze/:id` | Retrieve a cached analysis result by ID |

### Request Body (POST /api/analyze)

```json
{
  "url": "https://example.com/article",
  "text": "Optional raw text input if URL is not provided",
  "options": {
    "includeKeywords": true,
    "includeReadability": true,
    "includeTone": true
  }
}
```

### Response Format

```json
{
  "status": "success",
  "data": {
    "id": "analysis_abc123",
    "source": "https://example.com/article",
    "language": "en",
    "sentiment": {
      "label": "positive",
      "score": 0.72,
      "magnitude": 0.85
    },
    "subjectivity": {
      "label": "somewhat objective",
      "score": 0.35
    },
    "readability": {
      "fleschKincaid": 62.4,
      "gunningFog": 10.2,
      "smog": 9.8,
      "gradeLevel": "College"
    },
    "tone": {
      "primary": "analytical",
      "secondary": "formal",
      "confidence": 0.81
    },
    "keywords": [
      { "term": "machine learning", "relevance": 0.94, "count": 8 },
      { "term": "neural networks", "relevance": 0.87, "count": 5 }
    ]
  },
  "cached": false,
  "processingTime": 1240
}
```

### Error Response Format

```json
{
  "status": "error",
  "error": {
    "code": "EXTRACTION_FAILED",
    "message": "Unable to extract article content from the provided URL"
  }
}
```

## Getting Started

### Prerequisites

- Node.js 18 or later
- An NLP API key (MeaningCloud, Aylien, or compatible provider)

### Environment Variables

Create a `.env` file in the project root:

```
PORT=3000
NLP_API_KEY=your_nlp_api_key
NLP_PROVIDER=meaningcloud
CACHE_TTL=3600
RATE_LIMIT_WINDOW=15
RATE_LIMIT_MAX=100
```

### Installation

```bash
git clone https://github.com/abdomohamed911/article-intelligence-analyzer.git
cd article-intelligence-analyzer
npm install
```

### Run Locally

```bash
npm start
```

The API server starts at `http://localhost:3000`.

### Run Tests

```bash
npm test
```

### Run in Development Mode

```bash
npm run dev
```

Development mode enables hot reloading, detailed request logging, and expanded error messages.

## Use Cases

- **Content Strategy** -- Audit published articles for sentiment alignment, tone consistency, and readability before and after publication
- **Editorial Review** -- Automatically flag content that deviates from established brand voice guidelines
- **Competitive Analysis** -- Batch-process competitor articles to benchmark sentiment trends, keyword strategies, and content quality
- **Research Pipelines** -- Integrate as a preprocessing step in academic or market research workflows that require text analysis at scale
- **Content Automation** -- Wire into CMS workflows to trigger analysis on draft submission or publication events

## License

MIT

---

**Abdelrahman Mohamed** | [GitHub](https://github.com/abdomohamed911)
