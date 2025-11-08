# [💬 AI Real Estate Assistant App](https://ai-real-estate-assistant.streamlit.app/)

## Overview
The AI Real Estate Assistant is a conversational AI application designed to help real estate agencies assist potential buyers and renters in finding their ideal properties. The application uses natural language processing and machine learning to understand user preferences and recommend suitable properties from a database.

For detailed project requirements and specifications, see the [Product Requirements Document](docs/PRD.md).

## ✨ Latest Updates

### UI Modernization & Dark Mode (v2.1)
- **🌓 Dark Mode Support**: Professional dark theme with automatic system preference detection
- **🎨 WCAG 2.1 AA Compliance**: All text elements meet or exceed 4.5:1 contrast ratios
- **📱 Responsive Design**: Optimized for mobile, tablet, and desktop devices
- **♿ Accessibility**: Full keyboard navigation and screen reader support
- **🎭 Theme Toggle**: Manual override with persistent preference storage

## Solution Architecture & Design Decisions

### Technical Solution Overview

The AI Real Estate Assistant implements a Retrieval-Augmented Generation (RAG) system that combines the strengths of large language models with structured property data to provide intelligent property recommendations through natural conversation.

| Component | Implementation | Why We Chose It |
|-----------|----------------|-----------------|
| Large Language Models | OpenAI GPT / Llama | Provides natural language understanding and human-like responses with reasoning capabilities |
| Data Processing | Pandas | Industry-standard for handling tabular data with powerful querying and transformation features |
| Vector Embeddings | FastEmbed | Efficient, lightweight embedding generation for semantic search capabilities |
| Vector Storage | DocArrayInMemorySearch, ChromaDB | In-memory solution for quick retrieval in demonstration environments |
| Conversation Chain | ConversationalRetrievalChain | Maintains context across multiple turns of conversation with memory |
| Web Interface | Streamlit | Rapid development of interactive web applications with minimal code |

### System Workflow

1. **Data Loading & Preprocessing**:
   - CSV data is loaded and normalized using Pandas
   - Properties are transformed into vector embeddings using FastEmbed
   - Embeddings are stored in a vector database for efficient similarity search

2. **Conversation Handling**:
   - User queries are processed through the LLM to extract preferences
   - Preferences are used to query the property database
   - Results are formatted and presented in a conversational manner

3. **Retrieval Enhancement**:
   - V2 implements RAG to combine retrieved property information with LLM generation
   - This produces more relevant and accurate responses than pure LLM generation

## Features
- **Natural Language Conversation**: Engage users in a natural conversation about their property preferences
- **Property Search & Filtering**: Search properties based on location, budget, type, features, and more
- **Multiple Data Sources**: Support for local CSV files and external data sources via URLs
- **Flexible LLM Integration**: Support for OpenAI GPT models and open-source alternatives
- **Modern Dark Mode UI**: WCAG 2.1 AA compliant theming with system preference detection
- **Responsive Web Interface**: Easy-to-use Streamlit-based user interface optimized for all devices
- **Accessibility First**: Full keyboard navigation, screen reader support, and high contrast modes

## Strengths & Limitations

### Strengths
1. **Natural Interaction**: Users can express preferences in natural language without learning complex search interfaces
2. **Contextual Understanding**: System remembers previous interactions and builds cumulative understanding of user needs
3. **Flexible Integration**: Works with various data sources and LLM providers, avoiding vendor lock-in
4. **Explainable Recommendations**: Provides reasons why specific properties match user criteria
5. **Rapid Development**: Architecture allows fast iteration and feature addition

### Limitations
1. **Data Dependency**: Quality of recommendations depends heavily on the completeness and accuracy of property data
2. **Cold Start Challenges**: Without sufficient initial information, recommendations may be too broad
3. **Performance Issues**: Current implementation has a "db freeze" issue during heavy processing
4. **Limited Visualization**: Currently text-based with limited visual property representation
5. **Scaling Concerns**: In-memory vector store may face challenges with very large property databases

## Technology Stack
- **Frontend**: Streamlit
- **Backend**: Python (3.11+)
- **AI/ML**: LangChain, OpenAI GPT models, Llama models
- **Data Processing**: Pandas, FastEmbed
- **Vector Storage**: DocArrayInMemorySearch, ChromaDB
- **Package Management**: Poetry

## Project Structure
```
ai-real-estate-assistant/
├── src/                       # Source code
│   ├── ai/                   # AI agent implementation
│   │   ├── __init__.py
│   │   └── agent.py          # RealEstateGPT class
│   ├── common/               # Shared configuration
│   │   ├── __init__.py
│   │   └── cfg.py            # Configuration constants
│   ├── data/                 # Data loading modules
│   │   ├── __init__.py
│   │   └── csv_loader.py     # CSV utilities
│   ├── ui/                   # UI components and styling
│   │   ├── __init__.py
│   │   ├── theme_manager.py  # Dark mode theme management
│   │   └── styles.css        # WCAG-compliant CSS
│   └── utils/                # Utility functions
│       ├── __init__.py
│       ├── utils.py          # General utilities
│       └── streaming.py      # Streaming handlers
├── config/                   # Configuration files
│   └── streamlit/           # Streamlit config
│       └── config.toml      # Theme and app settings
├── docs/                     # Documentation
│   ├── images/              # Screenshots and diagrams
│   ├── PRD.md               # Product Requirements
│   └── TODO.md              # Project roadmap
├── dataset/                  # Sample datasets
│   └── pl/                  # Polish real estate data
├── scripts/                  # Utility scripts
│   ├── run_v1.sh            # Run V1 app
│   └── run_v2.sh            # Run V2 app
├── tests/                    # Test suite
├── app.py                    # V1 application entry
├── app_v2.py                 # V2 application entry (RAG)
├── pyproject.toml           # Poetry configuration
├── .gitignore               # Git ignore rules
├── README.md                # This file
├── CONTRIBUTING.md          # Contribution guidelines
├── CODE_OF_CONDUCT.md       # Code of conduct
└── DEVELOPMENT.md           # Development guide
```

## Project Versions

### V1: Pandas DataFrame Agent
- Used `langchain_experimental.agents.agent_toolkits.pandas.base.create_pandas_dataframe_agent`
- Basic property search functionality with single-turn conversations
- Run with: [run_v1.sh](run_v1.sh) | Application file: [app.py](app.py)

![V1 Screenshot](docs/images/screen.png)

### V2: RAG with Multiple LLM Support
- Uses different LLM models (OpenAI GPT, Llama)
- Implements RAG with `ConversationalRetrievalChain` for multi-turn conversations
- Enhanced property search and filtering capabilities
- Run with: [run_v2.sh](run_v2.sh) | Application file: [app_v2.py](app_v2.py)

![V2 Screenshot](docs/images/screen2.png)

## Future Development Roadmap

| Priority | Feature | Technical Approach | Expected Impact |
|----------|---------|-------------------|----------------|
| High | Fix "db freeze" issue | Implement asynchronous processing and background tasks | Improved user experience with responsive UI |
| High | Comprehensive testing suite | Add unit and integration tests with pytest | Enhanced reliability and easier maintenance |
| Medium | Property visualization features | Integrate image handling and comparison views | Better user understanding of property options |
| Medium | Multiple data source integration | Create adapters for RESTful APIs and additional file formats | Broader property selection and up-to-date data |
| Medium | Advanced filtering capabilities | Enhance vector search with multi-dimensional preference modeling | More precise matching of user preferences |
| Low | Market trend analysis | Add time-series analysis of property prices | Help users make better investment decisions |

### Addressing Current Limitations

1. **Performance Optimization**:
   - Implement connection pooling for database operations
   - Add request queuing system for high traffic scenarios
   - Optimize embedding generation with batching

2. **Enhanced User Experience**:
   - Add property comparison view for side-by-side evaluation
   - Implement proactive suggestions based on user history
   - Develop mobile-optimized interface

3. **Data Enrichment**:
   - Integrate with mapping services for location visualization
   - Add neighborhood statistics and amenity details
   - Include historical price trends and future projections

[//]: # (![demo.gif]&#40;assets/demo.gif&#41;)

## Architecture
The application follows a modular architecture with the following components:

1. **User Interface**: Streamlit-based web interface
2. **AI Agent**: LLM-powered conversational agent
3. **Data Processing**: CSV loaders and data formatters
4. **Data Storage**: In-memory database and vector stores

## Solution Comparison & Technical Considerations

### Implementation Evolution

| Aspect | V1 Implementation | V2 Implementation | Benefits of V2 |
|--------|-------------------|-------------------|---------------|
| Agent Type | Pandas DataFrame Agent | ConversationalRetrievalChain | Improved conversation memory and context retention |
| LLM Integration | Limited to OpenAI | Multiple model support (OpenAI, Llama) | Greater flexibility and cost options |
| Conversation Mode | Single-turn | Multi-turn with history | More natural conversation flow and follow-up questions |
| Result Quality | Basic data filtering | Enhanced reasoning with RAG | More relevant and personalized recommendations |
| Technical Complexity | Lower | Higher | More powerful but requires more careful implementation |

### Alternative Approaches Considered

| Approach | Pros | Cons | Why We Chose Our Approach |
|----------|------|------|---------------------------|
| Traditional Search Interface | Simpler implementation, faster queries | Limited understanding of complex preferences, requires user training | Our conversational approach provides better user experience and more intuitive interaction |
| Pure LLM Without RAG | Simpler architecture, fewer components | Hallucinations, less accurate property details | RAG provides factual grounding and prevents misinformation |
| SQL Database Only | Mature technology, optimized queries | Limited semantic understanding, rigid query structure | Vector database enables similarity search based on semantic meaning |
| External API Integration | Access to real-time data | Dependency on third-party services, potential costs | Local CSV processing provides control and predictability for demonstrations |

## Demo Highlights & Talking Points

When presenting this solution, focus on:

1. **Natural Conversation Flow**: Show how users can express complex requirements naturally
   - Example: "I need a 2-bedroom apartment in the city center with a parking space under $1500"

2. **Preference Refinement**: Demonstrate how the system narrows recommendations as users provide more details
   - Example: Start broad with "apartments in Warsaw" then refine with budget, amenities, etc.

3. **Context Retention**: Highlight how the assistant remembers previous statements
   - Example: User asks about parking after discussing a property, system knows which property they're referring to

4. **Reasoning Capabilities**: Show how it explains why properties match criteria
   - Example: "This property matches 90% of your criteria: it has the parking and 2 bedrooms you wanted, though it's slightly above your budget"

5. **Technical Architecture**: Briefly explain the RAG approach and how it prevents hallucinations

## Data Format
The application uses CSV datasets with the following structure. Datasets can be loaded from local files or remote URLs.
## DataFrame Columns

This table describes the columns in the DataFrame:

| Column Name              | Description                                |
|--------------------------|--------------------------------------------|
| `id`                     | Unique identifier for each record.         |
| `city`                   | Name of the city where the property is located. |
| `type`                   | Type of property (e.g., apartment, house). |
| `square_meters`          | Area of the property in square meters.     |
| `rooms`                  | Number of rooms in the property.           |
| `floor`                  | Floor number where the property is located. |
| `floor_count`            | Total number of floors in the building.    |
| `build_year`             | Year the building was constructed.         |
| `latitude`               | Latitude coordinate of the property.       |
| `longitude`              | Longitude coordinate of the property.      |
| `centre_distance`        | Distance from the property to the city center. |
| `poi_count`              | Number of Points of Interest nearby.       |
| `school_distance`        | Distance to the nearest school.            |
| `clinic_distance`        | Distance to the nearest clinic.            |
| `post_office_distance`   | Distance to the nearest post office.       |
| `kindergarten_distance`  | Distance to the nearest kindergarten.      |
| `restaurant_distance`    | Distance to the nearest restaurant.        |
| `college_distance`       | Distance to the nearest college.           |
| `pharmacy_distance`      | Distance to the nearest pharmacy.          |
| `ownership`              | Type of ownership (e.g., condominium).     |
| `building_material`      | Material used in the construction of the building. |
| `condition`              | Condition of the property (e.g., new, good). |
| `has_parking_space`      | Whether the property has a parking space (`True`/`False`). |
| `has_balcony`            | Whether the property has a balcony (`True`/`False`). |
| `has_elevator`           | Whether the building has an elevator (`True`/`False`). |
| `has_security`           | Whether the property has security (`True`/`False`). |
| `has_storage_room`       | Whether the property has a storage room (`True`/`False`). |
| `price`                  | Price of the property.                     |
| `price_media`            | Median price of similar properties.        |
| `price_delta`            | Difference between the property's price and `price_media`. |
| `negotiation_rate`       | Possibility of negotiation (e.g., High, Medium, Low). |
| `bathrooms`              | Number of bathrooms in the property.       |
| `owner_name`             | Name of the property owner.                |
| `owner_phone`            | Contact phone number of the property owner. |
| `has_garden`             | Whether the property has a garden (`True`/`False`). |
| `has_pool`               | Whether the property has a pool (`True`/`False`). |
| `has_garage`             | Whether the property has a garage (`True`/`False`). |
| `has_bike_room`          | Whether the property has a bike room (`True`/`False`). |



## Technical Deep Dive

### Key Implementation Challenges & Solutions

| Challenge | Solution | Technical Details |
|-----------|----------|-------------------|
| Extracting Structured Preferences from Unstructured Text | LLM-based entity extraction | Using function-calling capabilities of GPT models to convert natural language to structured parameters |
| Maintaining Conversation Context | ConversationBufferMemory | Storing and retrieving conversation history to inform subsequent responses |
| Efficient Property Retrieval | Vector similarity search | Converting property descriptions to embeddings and finding semantic similarity to user queries |
| Handling Multiple Data Formats | Standardized data schema | Normalizing diverse CSV formats to a consistent internal representation |
| Performance Optimization | Result caching and parallelization | Implementing caching strategies to avoid redundant computations |

### Code Architecture Principles

1. **Modularity**: Separation of concerns between data loading, AI processing, and user interface
   ```python
   # Example from data/csv_loader.py
   def load_csv_data(file_path: str) -> pd.DataFrame:
       """Load and normalize CSV data from a file path"""
       # Implementation follows Single Responsibility Principle
   ```

2. **Extensibility**: Designed for easy addition of new data sources or AI models
   ```python
   # Example from ai/agent.py
   class PropertyAgent:
       """Base agent class that can be extended with different LLM implementations"""
       # Implementation follows Open/Closed Principle
   ```

3. **Configuration Management**: Centralized configuration via environment variables and config files
   ```python
   # Example from common/cfg.py
   def get_api_key() -> str:
       """Retrieve API key from environment with appropriate fallbacks"""
       # Implementation follows best security practices
   ```

4. **Error Handling**: Robust error handling with graceful degradation
   ```python
   # Example strategy for handling API failures
   try:
       # Attempt primary model
   except Exception:
       # Fall back to alternative model or cached results
   ```

5. **Testing Strategy**: Unit tests for core components with integration testing for end-to-end flows

## Getting Started

### Prerequisites
- Python 3.11 or later
- [Poetry](https://python-poetry.org/) for dependency management
- OpenAI API Key (optional, for GPT models)

### Installation

#### 1. Clone the repository
```sh
git clone https://github.com/AleksNeStu/ai-real-estate-assistant.git
cd ai-real-estate-assistant
```

#### 2. Set up Poetry environment
```sh
# Install pip and poetry if needed
python -m ensurepip --upgrade
curl -sSL https://install.python-poetry.org | python3 - --version 1.7.0

# Configure Poetry
poetry config virtualenvs.in-project true
poetry config virtualenvs.prompt 'ai-real-estate-assistant'

# Install dependencies
poetry install --no-root
```

#### 3. Activate the virtual environment
```sh
source .venv/bin/activate
```

#### 4. Set up environment variables
Create a `.streamlit/secrets.toml` file in the project root:
```toml
OPENAI_API_KEY = "your-api-key-here"
```

**Note**: Never commit this file to version control! It's already in `.gitignore`.

### Running the Application

#### Local Development
Run either version of the application:
```sh
# Run V1 (Dataframe Agent)
streamlit run app.py
# OR use the script
./scripts/run_v1.sh

# Run V2 (RAG Implementation)
streamlit run app_v2.py
# OR use the script
./scripts/run_v2.sh
```

The application will automatically detect your system theme preference and apply the appropriate dark/light mode. You can manually toggle themes using the sidebar control.

For detailed development instructions, see [DEVELOPMENT.md](DEVELOPMENT.md).

### Deployment

#### Streamlit Cloud Deployment
This application can be deployed to Streamlit Cloud. For details, see the [Streamlit Deployment Documentation](https://docs.streamlit.io/deploy).

#### Docker Deployment
```sh
# Build and run Docker container
./utils/run_docker.sh
```

#### Development Container
```sh
# Run in development container
./utils/run_dev_container.sh
```

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details on:
- Setting up your development environment
- Coding standards and best practices
- Pull request process
- Testing guidelines

Please also read our [Code of Conduct](CODE_OF_CONDUCT.md) before contributing.

## Documentation

- **[Product Requirements](docs/PRD.md)** - Detailed project specifications
- **[Development Guide](DEVELOPMENT.md)** - Setup and development workflow
- **[Contributing Guidelines](CONTRIBUTING.md)** - How to contribute
- **[Code of Conduct](CODE_OF_CONDUCT.md)** - Community guidelines
- **[Project Roadmap](docs/TODO.md)** - Future plans and todos

## License
This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
