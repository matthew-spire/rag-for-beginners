# Introduction to uv

## Installing Python Versions
- Run `uv python install python-version`, where '`python-version`' is the version of Python you want to install (e.g., 3.12)

## Project Initialization and Setup
- Run `uv init project-name`, where '`project-name`' is the name of your project
- `pyproject.toml` is like `requirements.txt` but better &rarr; Allows you to configure everything about your project

## Creating Virtual Environments
- Run `uv venv --python python-version`, where, again, '`python-version`' is the version of Python you want to install (e.g., 3.12)
  - On Windows, activate the virtual environment by running `.venv\Scripts\activate`

## Package Management with uv
- Use `uv add package-name`, where `package-name` is the name of the package you want to add
  - Note: You can add multiple packages at once by appending additional `package-name`'s to the command
- Packages get added to `pyproject.toml`
  - If what is done via the command line and what is in `pyproject.toml` get out of sync, sync the two by running `uv sync`
- Removing packages, via the command line, can be done by running `uv remove package-name`, where, again, `package-name` is the name of the package you want to remove

## Running Python with uv
- Run `uv run file.py`, where `file.py` is the Python file you want to run
- Can use uv to run individual functions, which can be useful for building and debugging

# 3: Build Data Ingestion Pipeline with Python
- Run `uv add langchain langchain-community langchain_text_splitters langchain_openai langchain_chroma python_dotenv` to add all the necessary dependencies
- Store API keys in `.env` and then load using `load_dotenv`

## Load Documents
- `load_documents()`
- Check if the directory containing the documents exists, otherwise throw an error
- Load all the text (.txt) files from the docs directory
  - Note that the `glob` specifies what type of file(s) to look for and `loader_cls` indicates what loader to use &rarr; `glob` can be extended (an array?) to use other file types and `loader_cls` can include other types of loaders for different file types (.pdf, .ppt, etc.)
  - Needed to add `loader_kwargs={"encoding": "utf-8"}` to account for encoding
- Invoke the load method, which will give us a list (array) of LangChain Document(s)
  - Each LangChain Document will contain `page_content` and `metadata`, where `page_content` contains the entirety of what's in the (text) file and `metadata`, which is auto populated
  - `Document` is a LangChain type?
- Make sure that there were text files in the directory, otherwise throw an error
- Print some info from the first two documents
- Return the documents

## Chunk (Split) Documents
- `split_documents()`
- Splits documents into smaller chunks w/ overlap
- `chunk_size` is in characters
- `CharacterTextSplitter` is the most basic text splitting class that exists in LangChain
  - There are more advanced splitting methods
- Split the documents into chunks
- Observe how the docs have been split into chunks
- Return the chunks

## Create and Persist Vector Store
- Passing in the chunks and the location where we want the vector database to be created locally
- Initialize the embedding model
- Create the vector store
  - Takes all the chunks and creates the vector embeddings
  - Stores the vector embeddings in the vector store (database)
  - Specify the algorithm to be cosine similarity &rarr; This is basically what's being used to compare the user query to the vector embeddings (chunks)

## Add OpenAI API Key
- Make sure to add an OpenAI API key to the `.env` file