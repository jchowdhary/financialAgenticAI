# Financial AI Agent using Phidata

This project demonstrates an agentic AI workflow built with [Phidata](https://github.com/phidatahq/phidata) or [Agno]((https://github.com/agno-agi/agno), which enables the creation of AI agents with specific tools and capabilities. The application uses multiple AI agents to gather financial information by searching the web and retrieving stock data for any company.

## Overview

The Financial AI Agent combines two specialized agents:

1. **Web Search Agent**: Searches the web for up-to-date information about companies using DuckDuckGo.
2. **Finance Agent**: Retrieves detailed financial information including stock prices, analyst recommendations, fundamentals, and company news using YFinance.

These agents work together to provide comprehensive financial insights when you query about a specific company.

## Example Output

Below is an example of the agent's output when analyzing HCL Technologies:

![HCL Technologies Analysis Output](https://github.com/jchowdhary/financialAgenticAI/blob/main/screenshot.png)

*The image shows analyst recommendations from firms like Goldman Sachs, Morgan Stanley, and JPMorgan, along with recent news about HCL's partnerships and earnings reports.*

## Features

- Web search for company information with source citations
- Real-time stock price data
- Analyst recommendations
- Company fundamentals
- Latest company news
- Information presented in formatted tables
- Streaming responses

## Requirements

- Python 3.8+
- API keys for:
  - Phidata
  - GROQ
  - OpenAI

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/jchowdhary/financialAgenticAI.git
   cd financialAgenticAI

   ```

2. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Create a `.env` file in the root directory with your API keys:
   ```
   PHIDATA_API_KEY=your_phidata_key_here
   GROQ_API_KEY=your_groq_key_here
   OPENAI_API_KEY=your_openai_key_here
   ```

## Usage

Run the financial agent with:

```bash
python financial_agent.py
```

The script will execute a sample query about HCL Technologies, but you can modify the code to query any company of interest.

Example query:
```python
multi_ai_agent.print_response("Summarize analyst recommendation and share the latest news for Apple", stream=True)
```

## How It Works

The application creates two specialized agents:

```python
# Web search agent
web_search_agent = Agent(
    name="Web Search Agent",
    role="Search the web for the information",
    model=Groq(id="deepseek-r1-distill-llama-70b"),
    tools=[DuckDuckGo()],
    instructions=["Alway include sources"],
    show_tools_calls=True,
    markdown=True,
)

# Financial agent
finance_agent = Agent(
    name="Finance AI Agent",
    model=Groq(id="deepseek-r1-distill-llama-70b"),
    tools=[
        YFinanceTools(stock_price=True, analyst_recommendations=True, stock_fundamentals=True,
                      company_news=True),
    ],
    instructions=["Use tables to display the data"],
    show_tool_calls=True,
    markdown=True,
)
```

These agents are then combined into a multi-agent team:

```python
multi_ai_agent = Agent(
    team=[web_search_agent, finance_agent],
    instructions=["Always include sources", "Use table to display the data"],
    show_tool_calls=True,
    markdown=True,
)
```

## API Keys

- **GROQ**: Provides access to the models used by the agents. Visit [groq.com](https://groq.com) to obtain an API key.
- **OpenAI**: Used for additional AI capabilities. Visit [platform.openai.com](https://platform.openai.com) to get your API key.
- **Phidata**: Required for the agent framework. Visit [phidata.com](https://phidata.com) to obtain an API key.

## Notes

- GROQ provides access to various LLM models used in this project
- The `.env` file containing API keys should never be committed to version control
- For production use, consider implementing proper error handling and security measures

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
