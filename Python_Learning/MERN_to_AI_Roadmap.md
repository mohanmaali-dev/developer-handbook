# MERN Developer to AI Engineer Roadmap (Python + Generative AI)

> **Goal:** Move from MERN-stack development into AI engineering by
> learning Python, backend development, data and machine learning
> fundamentals, LLM application development, and production deployment.

This roadmap is designed for a developer who already knows JavaScript,
React, Node.js, Express, and databases. Your existing full-stack
experience is valuable: many AI engineering roles involve integrating
models into useful products, building APIs around them, managing data,
and shipping reliable applications---not necessarily inventing new AI
models.

**Estimated duration:** 6--8 months at around 1--2 focused hours per
day.\
**Recommended approach:** Learn a concept, build a small feature with
it, and then use it in a project. Avoid trying to master every library
before building anything.

------------------------------------------------------------------------

## 1. Understand the Target: What Does an AI Engineer Do?

An AI engineer builds software that uses AI models to solve real
problems. Typical work includes:

-   Connecting an application to hosted or locally running language
    models.
-   Designing prompts and requesting predictable, structured responses.
-   Giving models access to company or user data through
    retrieval-augmented generation (RAG).
-   Connecting models to tools such as APIs, databases, and search.
-   Building backend services that handle conversations, files, users,
    and model requests.
-   Evaluating output quality, controlling cost and latency, and
    monitoring failures.
-   Deploying and maintaining the complete application.

There are several related career paths:

  -----------------------------------------------------------------------
  Role                                Main focus
  ----------------------------------- -----------------------------------
  AI Application Engineer             Builds products using LLM APIs,
                                      RAG, tools, and application
                                      backends.

  Machine Learning Engineer           Trains, evaluates, and deploys
                                      machine-learning models.

  Research / Deep Learning Engineer   Works more deeply on model
                                      architectures, training methods,
                                      and research.

  Data Scientist                      Uses statistics and data to analyze
                                      problems and build predictive
                                      models.
  -----------------------------------------------------------------------

**A practical starting target for a MERN developer is AI Application
Engineer / GenAI Engineer.** You can later go deeper into machine
learning or model training if that becomes your goal.

------------------------------------------------------------------------

# Phase 1 --- Python Fundamentals

**Estimated time: 3--4 weeks**

Python is widely used in AI because its ecosystem includes libraries for
data processing, machine learning, deep learning, and model integration.
You do not need to forget JavaScript. Learn Python as another tool, and
compare its concepts with what you already know from Node.js.

## 1.1 Core syntax and data types

Learn:

-   Variables and basic types: `int`, `float`, `str`, `bool`, and
    `None`.
-   Type conversion and operators.
-   Conditional statements: `if`, `elif`, and `else`.
-   Loops: `for` and `while`.
-   Strings, slicing, and common string methods.
-   Lists, tuples, sets, and dictionaries.
-   Mutable vs. immutable objects.
-   List and dictionary comprehensions.

**Why it matters:** AI applications frequently manipulate lists of
messages, dictionaries containing model responses, file metadata, and
collections of retrieved documents.

## 1.2 Functions

Learn:

-   Defining and calling functions.
-   Positional and keyword arguments.
-   Default parameter values.
-   `*args` and `**kwargs`.
-   Return values and scope.
-   Lambda functions.
-   Basic decorators.

**JavaScript comparison:** Python's `**kwargs` collects keyword
arguments into a dictionary. It is useful when a function accepts
flexible named options.

## 1.3 Object-oriented programming

Learn:

-   Classes and objects.
-   `__init__` and instance attributes.
-   Instance methods and class methods.
-   Inheritance and composition.
-   Encapsulation and properties.
-   Special methods such as `__str__`.

You do not need to over-focus on advanced OOP initially. Understand how
to organize application code into reusable classes and modules.

## 1.4 Modules, packages, and environments

Learn:

-   Importing built-in and third-party modules.
-   Creating your own modules and packages.
-   Installing packages with `pip`.
-   Creating and activating a virtual environment with `venv`.
-   Managing dependencies with `requirements.txt`.
-   Basic familiarity with `pyproject.toml` and `uv`.

A virtual environment keeps one project's Python dependencies separate
from another project's dependencies.

Example setup:

``` bash
mkdir python-ai-roadmap
cd python-ai-roadmap

python3 -m venv .venv
source .venv/bin/activate

python -m pip install --upgrade pip
```

On Windows, activate it with:

``` powershell
.venv\Scripts\activate
```

## 1.5 Practical Python concepts

Learn:

-   Reading and writing files.
-   JSON serialization and deserialization.
-   Exception handling with `try`, `except`, and `finally`.
-   Type hints.
-   Basic logging.
-   Environment variables and `.env` files.
-   HTTP requests using `requests` or asynchronous `httpx`.

Never commit API keys or secrets to GitHub. Load them from environment
variables.

## 1.6 Asynchronous Python

Understand:

-   `async` and `await`.
-   Coroutines.
-   `asyncio`.
-   Concurrent I/O operations.
-   When synchronous code is sufficient and when asynchronous code
    helps.

**JavaScript comparison:** `async` / `await` will feel familiar, but
Python uses its own event-loop and coroutine model. Learn how to run and
coordinate asynchronous tasks rather than assuming the two runtimes
behave identically.

### Phase 1 project --- Public API CLI tool

Build a command-line program that:

1.  Accepts a search term or identifier from the terminal.
2.  Calls a public API using `requests` or `httpx`.
3.  Handles network errors and invalid responses.
4.  Converts the response into a useful Python data structure.
5.  Saves the result as a JSON file.
6.  Provides clear command-line output.

**Completion checklist**

-   [ ] Can create and activate a virtual environment.
-   [ ] Can write functions, classes, and modules.
-   [ ] Can read and write JSON and files.
-   [ ] Can call an HTTP API and handle errors.
-   [ ] Understands basic `async` / `await`.

------------------------------------------------------------------------

# Phase 2 --- Python Backend Development

**Estimated time: 2--3 weeks**

You already know Express.js, so this phase should be relatively quick.
The goal is to understand how to build a production-style Python API
that can later serve AI features.

## 2.1 FastAPI

FastAPI is a Python framework for building APIs. It supports
asynchronous endpoints, type-based request validation, and automatic API
documentation.

Learn:

-   Creating a FastAPI application.
-   GET, POST, PUT, PATCH, and DELETE endpoints.
-   Path parameters and query parameters.
-   Request bodies and response models.
-   Dependency injection.
-   Middleware and CORS.
-   Error handling and HTTP status codes.
-   Async endpoints.
-   API documentation through Swagger UI.

**Express comparison:**

  -----------------------------------------------------------------------
  Express.js                          FastAPI
  ----------------------------------- -----------------------------------
  `app.get()` / `app.post()`          `@app.get()` / `@app.post()`

  Middleware                          Middleware

  Request validation libraries        Pydantic models

  Route handlers                      Path operation functions

  OpenAPI setup often configured      OpenAPI documentation generated
  separately                          automatically
  -----------------------------------------------------------------------

## 2.2 Pydantic

Pydantic validates and parses data using Python type annotations.

Learn:

-   Defining `BaseModel` classes.
-   Required and optional fields.
-   Default values.
-   Field constraints.
-   Nested models.
-   Validation errors.
-   Serialization and response models.

For example, define a request model for a user registration endpoint and
let FastAPI validate incoming JSON.

## 2.3 Database integration

Start with PostgreSQL, especially if you are already learning SQL.

Learn:

-   Connecting FastAPI to PostgreSQL.
-   SQLAlchemy models and queries.
-   Database sessions and connection handling.
-   CRUD operations.
-   Relationships and joins.
-   Migrations with Alembic.
-   Transactions.
-   Basic indexing and query performance.

You can also work with MongoDB if a project requires it, but avoid
learning multiple database integrations at the same time.

## 2.4 Backend essentials

Learn:

-   Authentication and authorization.
-   Password hashing and token-based authentication.
-   Background tasks.
-   File uploads.
-   Configuration management.
-   Logging.
-   Testing API endpoints.
-   Docker fundamentals.

### Phase 2 project --- Rebuild a MERN API in FastAPI

Choose a small API you have already built with Node.js and Express.
Rebuild its backend in FastAPI.

Include:

-   CRUD endpoints.
-   Pydantic request validation.
-   PostgreSQL database integration.
-   Authentication.
-   Error handling.
-   API documentation.
-   Basic tests.

**Completion checklist**

-   [ ] Can create a FastAPI project.
-   [ ] Can validate request and response data with Pydantic.
-   [ ] Can connect to PostgreSQL using SQLAlchemy.
-   [ ] Can implement CRUD and authentication.
-   [ ] Can run the API locally and document its endpoints.

------------------------------------------------------------------------

# Phase 3 --- Data Handling and Math Essentials

**Estimated time: 3--4 weeks**

This phase provides the foundation for understanding datasets and
machine-learning concepts. You do not need advanced mathematics before
starting LLM application development, but basic data and math knowledge
will make later topics easier.

## 3.1 NumPy

NumPy is used for numerical computing.

Learn:

-   Arrays and array creation.
-   Array shapes and dimensions.
-   Indexing and slicing.
-   Reshaping arrays.
-   Vectorized operations.
-   Broadcasting.
-   Basic aggregation.
-   Dot products.

**Why it matters:** Many machine-learning operations work with numerical
arrays and vectors. Embeddings, for example, are represented as vectors
of numbers.

## 3.2 Pandas

Pandas helps work with structured data.

Learn:

-   Series and DataFrames.
-   Reading CSV and Excel files.
-   Selecting rows and columns.
-   Filtering and sorting.
-   Handling missing values.
-   Grouping and aggregation.
-   Merging datasets.
-   Basic data cleaning.

## 3.3 Matplotlib

Learn how to visualize data with:

-   Line charts.
-   Bar charts.
-   Histograms.
-   Scatter plots.
-   Labels, titles, and legends.

Focus on understanding what a chart tells you rather than learning every
plotting option.

## 3.4 Math intuition

Study the following at a practical level:

**Linear algebra** - Scalars, vectors, and matrices. - Dimensions and
shapes. - Vector addition and multiplication. - Dot product. - Cosine
similarity.

**Statistics and probability** - Mean, median, and standard deviation. -
Distributions. - Probability basics. - Conditional probability. -
Correlation vs. causation. - Sampling and data variation.

**Calculus and optimization** - What a derivative represents. - Gradient
intuition. - Loss functions. - Gradient descent.

You should understand what these ideas mean and where they are used.
Detailed mathematical proofs are not necessary at this stage.

## 3.5 Jupyter Notebook

Learn to use Jupyter notebooks to:

-   Run Python code in small cells.
-   Explore datasets.
-   Test calculations.
-   Visualize results.
-   Document experiments.

### Phase 3 project --- Dataset analysis

Choose a dataset from Kaggle or another public source.

Your task:

1.  Load the dataset using Pandas.
2.  Inspect its columns, types, and missing values.
3.  Clean invalid or missing data.
4.  Calculate useful statistics.
5.  Find patterns or relationships.
6.  Create visualizations using Matplotlib.
7.  Write a short conclusion explaining what you discovered.

**Completion checklist**

-   [ ] Can manipulate NumPy arrays.
-   [ ] Can clean and analyze a DataFrame.
-   [ ] Understands basic vectors and matrices.
-   [ ] Understands basic probability and statistics.
-   [ ] Can create and explain simple visualizations.

------------------------------------------------------------------------

# Phase 4 --- Machine Learning Fundamentals

**Estimated time: 4--6 weeks**

Machine learning (ML) is the process of enabling a system to learn
patterns from data and use those patterns to make predictions or
decisions.

You do not need to become a research-level ML engineer to build LLM
applications. However, learning the fundamentals will help you
understand model behavior, evaluation, and the wider AI ecosystem.

## 4.1 Main machine-learning categories

**Supervised learning**

The model learns from examples that contain the expected answer (label).

Examples: - Predicting a house price. - Classifying an email as spam or
not spam. - Predicting whether a customer may leave a service.

Common tasks: - Regression: predict a numerical value. - Classification:
predict a category.

**Unsupervised learning**

The data does not contain target labels. The algorithm tries to discover
patterns or groups.

Examples: - Grouping customers by behavior. - Finding similar
products. - Discovering clusters in a dataset.

**Reinforcement learning**

An agent learns by taking actions and receiving rewards or penalties
from an environment. Learn the basic idea first; advanced reinforcement
learning can wait.

## 4.2 Essential ML concepts

Learn:

-   Features and labels.
-   Training, validation, and test datasets.
-   Train/test split.
-   Data preprocessing.
-   Overfitting and underfitting.
-   Bias and variance intuition.
-   Feature scaling.
-   Model evaluation.
-   Data leakage.
-   Cross-validation basics.

Understand why a model can perform well on training data but poorly on
unseen data.

## 4.3 Evaluation metrics

For regression, learn: - MAE (Mean Absolute Error). - MSE (Mean Squared
Error). - RMSE (Root Mean Squared Error).

For classification, learn: - Accuracy. - Precision. - Recall. -
F1-score. - Confusion matrix. - ROC-AUC at a basic level.

Do not rely only on accuracy. For example, a fraud-detection model can
achieve high accuracy by predicting that almost every transaction is
legitimate, while missing most actual fraud.

## 4.4 Scikit-learn

Start with scikit-learn. Practice:

-   Linear regression.
-   Logistic regression.
-   Decision trees.
-   Random forests.
-   K-nearest neighbors.
-   K-means clustering.
-   Preprocessing pipelines.
-   Model evaluation.

Learn the common workflow:

1.  Load and inspect data.
2.  Separate features and labels.
3.  Split the dataset.
4.  Preprocess the data.
5.  Train a model.
6.  Evaluate it on unseen data.
7.  Save and load the trained model.

## 4.5 Neural networks and PyTorch

First understand:

-   Neurons and layers.
-   Weights and biases.
-   Activation functions.
-   Forward pass.
-   Loss functions.
-   Backpropagation intuition.
-   Optimizers and training epochs.

Then start PyTorch:

-   Tensors.
-   Dataset and DataLoader.
-   Building a basic neural network.
-   Loss functions and optimizers.
-   Training and evaluation loops.
-   Saving and loading model weights.

### Learning resources

-   [Andrew Ng --- Machine Learning
    Specialization](https://www.coursera.org/specializations/machine-learning-introduction)
-   [fast.ai --- Practical Deep Learning for
    Coders](https://course.fast.ai/)
-   [Karpathy --- Neural Networks: Zero to
    Hero](https://karpathy.ai/zero-to-hero.html)
-   [scikit-learn documentation](https://scikit-learn.org/stable/)
-   [PyTorch tutorials](https://pytorch.org/tutorials/)

### Phase 4 project --- Prediction API

Build a simple classifier or regression model.

Example: spam detection.

1.  Obtain and clean a labeled dataset.
2.  Convert text into numerical features.
3.  Train a baseline model with scikit-learn.
4.  Evaluate it using suitable metrics.
5.  Save the model.
6.  Load it in a FastAPI application.
7.  Create an endpoint that accepts input and returns a prediction.
8.  Add validation and error handling.

**Completion checklist**

-   [ ] Understands supervised and unsupervised learning.
-   [ ] Can split data and avoid basic data leakage.
-   [ ] Can train and evaluate a scikit-learn model.
-   [ ] Understands basic neural-network concepts.
-   [ ] Has used PyTorch at least once.

------------------------------------------------------------------------

# Phase 5 --- LLM and Generative AI Engineering

**Estimated time: 6--8 weeks**

This is the most important phase if your target is AI application
development.

A large language model (LLM) is trained to process and generate
language. As an AI engineer, you will usually integrate an existing
model into an application and build the surrounding system: prompts,
data retrieval, tools, APIs, evaluation, and user experience.

## 5.1 Understand how LLMs work

Learn these concepts before relying heavily on frameworks:

**Tokens**

Models process text as tokens, not necessarily complete words. Token
count affects context usage, response length, and API cost.

**Embeddings**

An embedding is a numerical vector representing the meaning or
characteristics of content. Similar content often has vectors that are
close in vector space.

**Transformers and attention**

Learn the high-level ideas:

-   What a transformer is.
-   Why attention is useful.
-   What self-attention does.
-   How tokens interact with surrounding context.
-   Why transformers can process language effectively.

You do not need to implement a transformer from scratch to build useful
LLM applications.

**Context window**

The context window is the amount of tokenized information a model can
consider in a request. It includes relevant input and, depending on the
model/API, output constraints and conversation context.

**Temperature and generation settings**

Understand how generation settings affect response variability. Do not
assume that a higher temperature always means a better response.

## 5.2 Prompt engineering and structured output

Learn how to:

-   Give the model a clear task.
-   Provide relevant context.
-   Set boundaries and constraints.
-   Include examples when useful.
-   Separate instructions from user-provided data.
-   Request JSON or schema-constrained output.
-   Validate the response before using it.
-   Handle refusals, incomplete responses, and malformed output.

A prompt is not a replacement for application validation or security
checks. Treat model output as untrusted input.

## 5.3 Work with model APIs

Start with one provider's official SDK and learn the request/response
lifecycle.

Explore: - OpenAI API. - Anthropic API. - Open-source models through
Hugging Face. - Local model runtimes such as Ollama.

Learn: - Authentication and secret management. - Sending messages and
reading responses. - Streaming responses. - Token usage and cost. -
Timeouts and retries. - Rate limits. - Error handling. - Structured
outputs. - Tool/function calling.

Do not try to master every provider at once. Learn one API well, then
compare providers.

Useful documentation: - [OpenAI API
documentation](https://platform.openai.com/docs) - [Anthropic
documentation](https://docs.anthropic.com/) - [Hugging Face
documentation](https://huggingface.co/docs) - [Ollama
documentation](https://docs.ollama.com/)

## 5.4 Vector databases and similarity search

When you convert text into embeddings, you need a way to store and
search those vectors.

Learn:

-   Creating embeddings.
-   Vector dimensions.
-   Similarity search.
-   Cosine similarity.
-   Metadata filtering.
-   Approximate nearest-neighbor search.
-   Updating and deleting vectors.

Explore one option first:

-   PostgreSQL with [pgvector](https://github.com/pgvector/pgvector).
-   [Qdrant](https://qdrant.tech/documentation/).
-   [Chroma](https://docs.trychroma.com/).
-   [Pinecone](https://docs.pinecone.io/).

You do not need to learn all of them. Pick one based on your project's
requirements.

## 5.5 RAG --- Retrieval-Augmented Generation

RAG allows an LLM application to retrieve relevant information from an
external knowledge source and use that information when generating an
answer.

A typical RAG flow:

1.  **Collect:** Accept documents such as PDFs, text files, or web
    content.
2.  **Extract:** Read the text from each document.
3.  **Clean:** Remove irrelevant or malformed content.
4.  **Chunk:** Split long documents into smaller, meaningful sections.
5.  **Embed:** Convert each chunk into a vector.
6.  **Store:** Save vectors and metadata in a vector database.
7.  **Retrieve:** Convert the user's question into an embedding and
    search for relevant chunks.
8.  **Build context:** Select and format the retrieved content.
9.  **Generate:** Send the question and retrieved context to the LLM.
10. **Respond:** Return the answer, ideally with source references.

Important RAG topics:

-   Chunk size and overlap.
-   Metadata and document permissions.
-   Top-k retrieval.
-   Similarity thresholds.
-   Hybrid search (keyword + vector search).
-   Reranking.
-   Citations and source attribution.
-   Retrieval evaluation.
-   Handling questions when no useful context is found.

**Important:** RAG does not automatically make an answer correct. Poor
extraction, chunking, retrieval, or prompting can still produce
incorrect answers. Test each stage.

## 5.6 Tool calling and AI agents

A model can request that your application run a tool, such as a database
query or an external API call. Your application executes the approved
operation and returns the result to the model.

Learn tool calling before building complex autonomous agents.

Example tools: - Search products. - Check an order status. - Query a
database. - Create a support ticket. - Retrieve a user's project
information.

Understand:

-   Tool definitions and schemas.
-   Model-generated tool requests.
-   Application-side execution.
-   Returning tool results to the model.
-   Multi-step tool workflows.
-   Permission checks.
-   Timeouts and failure handling.
-   Human approval for sensitive actions.

An agent is not simply a chatbot with a long prompt. It is a system that
can decide which steps or tools to use within defined boundaries.

## 5.7 Learn frameworks after raw APIs

Once you understand model calls, embeddings, retrieval, and tool
calling, explore:

-   LangChain.
-   LangGraph.
-   LlamaIndex.

Frameworks can simplify orchestration, but they do not replace
understanding the underlying API calls and data flow.

## 5.8 Evaluation, guardrails, and reliability

Learn to test more than whether the endpoint returns HTTP 200.

Evaluate: - Is the answer relevant? - Is it supported by retrieved
sources? - Did retrieval find the right information? - Did the model
call the correct tool? - Did it follow the required output schema? - How
often does it hallucinate? - What are the latency and token costs?

Also learn: - Prompt injection risks. - Input and output validation. -
Data access controls. - Rate limiting. - Sensitive-data handling. - Safe
tool permissions. - Logging without exposing secrets or private content.

## 5.9 Fine-tuning basics

Understand what fine-tuning is and when it may help.

Learn the high-level ideas behind: - Training data preparation. -
Supervised fine-tuning. - LoRA. - PEFT. - Training and evaluation
splits.

**Do not jump into fine-tuning immediately.** First check whether prompt
improvements, better retrieval, tool calling, or a different model solve
the problem. Fine-tuning is not a substitute for giving a model the
correct current information.

### Phase 5 projects

Build these in increasing difficulty.

**Project 1 --- Chat with your PDFs (RAG)**

Features: - Upload PDF documents. - Extract and chunk text. - Generate
embeddings. - Store chunks in a vector database. - Ask questions about
uploaded documents. - Show relevant source pages or references. - Handle
questions that are not answered by the documents.

**Project 2 --- Tool-using AI assistant**

Features: - Connect an LLM to a small set of tools. - Include at least
one API tool and one database tool. - Validate tool arguments. - Check
user permissions before execution. - Handle tool errors and timeouts. -
Keep a trace of the steps taken.

**Project 3 --- Full-stack AI application**

Suggested architecture:

``` text
React Frontend
      |
      v
FastAPI Backend
      |
      +------ LLM Provider
      |
      +------ PostgreSQL
      |
      +------ Vector Database
      |
      +------ File Storage
```

Features: - User authentication. - Chat interface. - Streaming
responses. - Conversation history. - Document upload. - RAG retrieval. -
Source references. - Error and loading states. - Usage limits and basic
cost tracking.

**Completion checklist**

-   [ ] Can call an LLM API directly.
-   [ ] Understands tokens, embeddings, and context windows.
-   [ ] Can request and validate structured output.
-   [ ] Can build a basic RAG pipeline.
-   [ ] Can store and retrieve embeddings.
-   [ ] Can implement tool calling with permission checks.
-   [ ] Can evaluate output quality and handle common failures.

------------------------------------------------------------------------

# Phase 6 --- Production and MLOps Basics

**Estimated time: 3--4 weeks**

A working demo is not necessarily a production-ready application.
Production engineering is about making the system reliable, secure,
observable, and cost-effective.

## 6.1 Deployment and infrastructure

Learn:

-   Docker images and containers.
-   Docker Compose.
-   Environment-based configuration.
-   CI/CD fundamentals.
-   Linux server basics.
-   Deploying FastAPI.
-   Deploying React.
-   Managed PostgreSQL.
-   Cloud fundamentals on AWS, GCP, or Azure.

Choose one cloud provider initially. You do not need to learn all three.

## 6.2 Performance and cost

Learn:

-   Caching repeated results.
-   Rate limiting.
-   Request timeouts.
-   Retry strategies and exponential backoff.
-   Streaming.
-   Batching where appropriate.
-   Token and usage tracking.
-   Choosing a model based on task complexity.
-   Reducing unnecessary context.
-   Controlling embedding and retrieval costs.

## 6.3 Observability

Learn how to understand what happened during an AI request.

Track: - Request IDs. - Model and model version. - Latency. - Token
usage. - Tool calls. - Retrieval results. - Errors and retries. -
Evaluation results.

Explore: - [Langfuse](https://langfuse.com/docs) -
[LangSmith](https://docs.smith.langchain.com/)

Be careful not to log API keys, passwords, or sensitive user data.

## 6.4 Model serving basics

Learn what model serving means and when self-hosting is useful.

Explore at a high level: - Hugging Face inference. - vLLM. - GPU memory
and batching. - Throughput vs. latency. - Hosted APIs vs. self-hosted
models.

You do not need to operate GPU infrastructure before you can build LLM
applications. Learn the trade-offs first.

### Phase 6 project --- Production deployment

Take your full-stack AI application and:

1.  Dockerize the backend and supporting services.
2.  Configure secrets through environment variables.
3.  Add request validation, rate limits, and timeouts.
4.  Add logs and request tracing.
5.  Track token usage and approximate cost.
6.  Deploy the application.
7.  Write setup and deployment instructions.
8.  Test failure cases, not just the happy path.

**Completion checklist**

-   [ ] Can Dockerize a FastAPI service.
-   [ ] Can deploy a full-stack application.
-   [ ] Understands basic caching and rate limiting.
-   [ ] Can track latency, errors, and model usage.
-   [ ] Has considered security and operational failure cases.

------------------------------------------------------------------------

# Phase 7 --- Portfolio and Job Preparation

**Estimated time: Ongoing**

Your portfolio should demonstrate that you can solve real problems, not
just follow tutorials.

## 7.1 Build 3--4 strong projects

A good portfolio could contain:

  -----------------------------------------------------------------------
  Project                             What it demonstrates
  ----------------------------------- -----------------------------------
  PDF Knowledge Assistant             Document processing, embeddings,
                                      RAG, citations

  AI Tool-Using Assistant             Tool calling, API integration,
                                      permissions, workflows

  Full-Stack AI SaaS                  React, FastAPI, authentication,
                                      streaming, database design

  ML Prediction API                   Data processing, model training,
                                      evaluation, model serving
  -----------------------------------------------------------------------

It is better to have two polished, explainable projects than many
unfinished tutorial clones.

## 7.2 Make each GitHub repository professional

Include:

-   A clear project description.
-   The problem it solves.
-   Architecture diagram.
-   Tech stack.
-   Main features.
-   Setup instructions.
-   Environment variable example file (without real secrets).
-   API documentation.
-   Screenshots or a short demo.
-   Known limitations.
-   Future improvements.

## 7.3 Prepare for interviews

Revise your existing strengths:

**JavaScript and backend** - JavaScript fundamentals. - Node.js event
loop. - Async programming. - REST API design. - Authentication and
authorization. - Error handling. - Database design. - Performance and
caching.

**Python** - Data structures. - Functions and OOP. - Exceptions. -
Iterators and generators. - Async programming. - FastAPI and Pydantic.

**SQL and databases** - Joins. - Aggregations. - Subqueries. -
Indexes. - Transactions. - Normalization. - Query optimization.

**AI engineering** - LLM request lifecycle. - Tokens and embeddings. -
RAG architecture. - Chunking and retrieval. - Vector databases. - Tool
calling. - Hallucinations and evaluation. - Prompt injection and
security. - Cost and latency trade-offs.

**System design** - Design a document Q&A system. - Design a multi-user
AI chat application. - Design a tool-using assistant. - Explain how to
secure user-specific documents in RAG. - Explain how to monitor and
reduce LLM costs.

------------------------------------------------------------------------

# Suggested Timeline

The timeline is approximate. It assumes around 1--2 focused hours per
day and some extra time for project work.

  Phase   Focus                                   Estimated duration
  ------- ------------------------------------- --------------------
  1       Python fundamentals                             3--4 weeks
  2       FastAPI and backend                             2--3 weeks
  3       Data and math essentials                        3--4 weeks
  4       Machine learning basics                         4--6 weeks
  5       LLM and GenAI engineering                       6--8 weeks
  6       Production and MLOps                            3--4 weeks
  7       Portfolio and interview preparation                Ongoing

**Total:** Approximately 6--8 months, depending on consistency, prior
knowledge, and project complexity.

Do not treat the timeline as a strict deadline. Understanding and
building matter more than finishing a phase quickly.

------------------------------------------------------------------------

# A More Direct Route If Your Goal Is LLM Application Development

If you mainly want to become an AI Application Engineer, you can adjust
the order:

1.  Learn Python fundamentals.
2.  Learn FastAPI and Pydantic.
3.  Learn enough SQL and NumPy to work comfortably with data.
4.  Start calling LLM APIs directly.
5.  Learn embeddings and vector search.
6.  Build a RAG application.
7.  Learn tool calling and agent workflows.
8.  Learn evaluation, security, and production deployment.
9.  Study deeper ML and PyTorch alongside your projects.

You should still learn basic ML concepts, but you do not need to finish
an extensive ML curriculum before building your first LLM application.

------------------------------------------------------------------------

# Recommended Daily Study Routine

For a 1--2 hour study session:

  Time             Activity
  ---------------- --------------------------------------
  15--20 minutes   Review yesterday's concepts
  25--35 minutes   Learn one new concept
  30--50 minutes   Write code and build a small feature
  5--10 minutes    Write notes and record questions

A useful rule: **spend more time coding than watching tutorials.**

At the end of each week, try to build something without copying a
tutorial step by step.

------------------------------------------------------------------------

# Recommended Learning Rules

1.  **Do not abandon your MERN experience.** React remains useful for AI
    product interfaces, and backend/API knowledge transfers directly.
2.  **Learn raw APIs before frameworks.** Understand model calls,
    retrieval, and tool execution before depending on LangChain or
    similar tools.
3.  **Do not over-study mathematics at the beginning.** Learn the
    intuition needed for the project you are building.
4.  **Build incrementally.** Start with one API call, then structured
    output, then retrieval, then tools, then a complete application.
5.  **Learn one tool deeply before comparing alternatives.** For
    example, use PostgreSQL with pgvector before trying several vector
    databases.
6.  **Treat model output as untrusted.** Validate it and enforce
    authorization in your backend.
7.  **Measure quality.** Keep a small test set of questions and check
    whether changes improve the answers.
8.  **Understand the trade-offs.** Be ready to explain why you chose a
    model, database, chunk size, or retrieval strategy.
9.  **Keep projects deployable.** A working live demo and clear README
    make your skills easier to evaluate.
10. **Keep learning current documentation.** AI APIs and model
    capabilities change quickly; verify current SDK patterns and model
    availability when implementing a project.

------------------------------------------------------------------------

# Final Roadmap Summary

Your transition can be thought of as three connected skill groups:

**1. Software engineering foundation** - Python - FastAPI - PostgreSQL -
APIs - Authentication - Docker

**2. AI foundation** - Data handling - Basic statistics and linear
algebra - Machine learning concepts - Neural-network fundamentals

**3. AI application engineering** - LLM APIs - Prompting and structured
output - Embeddings and vector search - RAG - Tool calling and agents -
Evaluation and guardrails - Deployment, monitoring, and cost
optimization

The most practical goal is to become a strong software engineer who can
design, build, evaluate, and deploy AI-powered applications. Your MERN
experience gives you a head start on the product and backend side;
Python and the AI ecosystem are the next layers to add.
