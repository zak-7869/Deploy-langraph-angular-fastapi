# Research AI — Web Research & Summarization

**Web research, simplified.**

Research AI is a full-stack web application that researches a topic using web search, processes the retrieved information with AI, and generates a structured research report with source links.

## 1. Project Overview

The application combines an Angular frontend, FastAPI backend, LangGraph workflow orchestration, Tavily web search, and Groq language models.

### Key Features

* Topic-based web research
* AI-generated research reports and summaries
* Source links for further reading
* Responsive Angular user interface
* FastAPI REST API
* LangGraph multi-step workflow
* Error handling and request validation
* Separate frontend and backend deployments on Vercel

## 2. Technology Stack

| Component          | Technology               |
| ------------------ | ------------------------ |
| Frontend           | Angular, TypeScript, CSS |
| HTTP communication | Angular HttpClient       |
| Backend            | Python, FastAPI          |
| AI workflow        | LangGraph                |
| Language model     | Groq, through LangChain  |
| Web search         | Tavily Search API        |
| Deployment         | Vercel                   |

## 3. System Architecture

The application follows a frontend-backend architecture.

1. The user enters a research topic in Angular.
2. Angular sends a POST request to FastAPI.
3. FastAPI validates the topic and invokes the LangGraph workflow.
4. The research node retrieves web results using Tavily.
5. The summarization node uses Groq to generate a structured report.
6. FastAPI returns the report and source links as JSON.
7. Angular displays the results.

## 4. Project Structure

```text
research-summarizer/
├── frontend/
│   ├── src/
│   │   └── app/
│   ├── angular.json
│   ├── package.json
│   └── vercel.json
├── backend/
│   ├── app/
│   │   └── main.py
│   ├── api/
│   │   └── index.py
│   ├── requirements.txt
│   └── vercel.json
├── docs/
│   ├── architecture-diagrams.md
│   └── research-workflow.mmd
├── .gitignore
└── README.md
```

*Keep your existing generated Angular files and Python package structure if their names differ.*

## 5. Prerequisites

Install the following tools:

* Node.js and npm
* Python
* Git
* A Groq API key
* A Tavily API key

## 6. Local Installation

### Backend Setup

Open a terminal in the `backend` directory.

Create a virtual environment:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install the Python dependencies:

```powershell
pip install -r requirements.txt
```

Create a `backend/.env` file:

```dotenv
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
GROQ_MODEL=openai/gpt-oss-120b
FRONTEND_ORIGIN=http://localhost:4200
```

Start the backend:

```powershell
uvicorn app.main:app --reload --port 8000
```

API documentation:

http://127.0.0.1:8000/docs

### Frontend Setup

Open another terminal in the `frontend` directory.

Install dependencies:

```powershell
npm install
```

Start Angular:

```powershell
npm start
```

If `npm start` is not configured, use:

```powershell
npx ng serve
```

Open the application:

http://localhost:4200

## 7. Environment Variables

| Variable          | Purpose                                        |
| ----------------- | ---------------------------------------------- |
| `GROQ_API_KEY`    | Authenticates with Groq                        |
| `TAVILY_API_KEY`  | Authenticates with Tavily                      |
| `GROQ_MODEL`      | Specifies the language model                   |
| `FRONTEND_ORIGIN` | Defines the permitted frontend origin for CORS |

**Security:** Never commit `.env` files or expose API keys in Angular code.

## 8. API Documentation

### POST `/api/research`

Generates a research report for the supplied topic.

Example request:

```json
{
  "topic": "Recent advances in renewable energy"
}
```

Expected response structure:

```json
{
  "topic": "Recent advances in renewable energy",
  "report": "Generated research report...",
  "sources": [
    {
      "title": "Example article",
      "url": "https://example.com/article"
    }
  ]
}
```

The example is illustrative; actual report content and sources depend on the search results and model response.

## 9. Production Deployment

### Backend

Deploy the backend as a separate Vercel project.

* Root Directory: `backend`
* Configure the Python entrypoint and dependencies.
* Add the required environment variables.
* Deploy and verify the API endpoints.

### Frontend

Deploy the Angular application as a separate Vercel project.

* Root Directory: `frontend`
* Build Command: `npm run build`
* Output Directory: confirm the actual Angular browser output path after building.
* Ensure the frontend calls the deployed backend URL.

### Production URLs

**Frontend:**
https://research-summarizer-frontend.vercel.app/

**Backend:**
https://research-summarizer.vercel.app/

The production Angular request must use:

```typescript
'https://research-summarizer-api.vercel.app/api/research'
```

It must not use `http://127.0.0.1:8000` in the deployed application.

## 10. CORS Configuration

Set the backend environment variable in Vercel:

```text
FRONTEND_ORIGIN=https://research-summarizer-frontend.vercel.app
```

Configure FastAPI CORS middleware to allow this exact origin and the methods and headers required by the application. Redeploy the backend after changing environment variables.

CORS controls browser access; it is not a replacement for authentication or API security.

## 11. Troubleshooting

| Problem                    | Recommended check                                                  |
| -------------------------- | ------------------------------------------------------------------ |
| Frontend returns 404       | Verify Vercel root directory and Angular output directory          |
| API returns 404            | Verify the backend entrypoint and API route                        |
| Browser reports CORS error | Verify `FRONTEND_ORIGIN` and the request URL                       |
| API returns 500            | Inspect backend logs and provider configuration                    |
| Research remains loading   | Inspect the browser Network tab and Vercel function logs           |
| Groq request fails         | Check the API key, model availability, quotas, and provider errors |
| Tavily request fails       | Check the API key, quota, and search request                       |

## 12. Production Best Practices

* Validate and limit incoming topic length.
* Apply timeouts to external API calls.
* Return safe, actionable errors to the client.
* Log failures and execution times without exposing secrets.
* Limit search-result count and retrieved content size.
* Protect the public API with suitable rate limiting.
* Keep dependencies updated.
* Add automated tests for API routes and workflow nodes.
* Sanitize generated HTML if rendering AI output as HTML.
* Monitor provider quotas and deployment execution limits.

## 13. Future Enhancements

* Streaming research progress
* Downloadable PDF and Markdown reports
* User accounts and saved research history
* Source relevance scoring
* Citation verification
* Research report export
* Automated testing and CI/CD
* Background jobs for long-running research tasks

## License

Add a license appropriate for your intended use before distributing the project.
