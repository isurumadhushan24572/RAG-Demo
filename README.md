# 🎫 AI-Powered Service Management System with RAG

A prototype service management system that uses Retrieval-Augmented Generation (RAG) to provide intelligent solutions for IT service tickets based on historical data.

## 🌟 Features

### 1. **Ticket Management**
- Submit new service tickets with title, description, and category
- Automatic ticket ID generation (TKT-0001, TKT-0002, etc.)
- Track ticket status (Open/Resolved)
- Categorize tickets: Hardware, Software, Network, Security, Access, Performance, Other

### 2. **RAG-Powered Solution Generation**
- Uses FAISS vector database to store embeddings of historical tickets
- Finds similar past tickets using semantic search
- Generates intelligent solutions based on:
  - Current ticket description
  - Similar past tickets and their solutions
  - AI reasoning using OpenAI GPT-4

### 3. **Intelligent Analysis**
- **Reasoning**: AI explains why the issue is occurring
- **Solution**: Step-by-step resolution guide
- **Similar Tickets**: Shows related past tickets with similarity scores

### 4. **Comprehensive Dashboard**
- View all tickets with filters (status, category)
- Statistics dashboard (total tickets, open/resolved, resolution rate)
- Detailed ticket view with full history
- Manual solution override option

## 📋 Requirements

- Python 3.8+
- OpenAI API key
- Required packages (see `requirements.txt`)

## 🚀 Installation

1. **Clone or download the project**

2. **Install dependencies**:
```bash
pip install -r requirements.txt
```

3. **Set up environment variables**:
Create a `.env` file in the project root:
```
OPENAI_API_KEY=your_openai_api_key_here
```

## 💻 Usage

1. **Start the application**:
```bash
streamlit run app.py
```

2. **Submit a New Ticket**:
   - Navigate to "Submit New Ticket"
   - Enter ticket title and detailed description
   - Select appropriate category
   - Click "Submit Ticket and Get AI Solution"
   - System will:
     - Search for similar past tickets
     - Generate AI-powered reasoning and solution
     - Save ticket to knowledge base

3. **View All Tickets**:
   - Navigate to "View All Tickets"
   - Filter by status (All/Open/Resolved) or category
   - View statistics dashboard
   - Click on any ticket to see details

4. **View Ticket Details**:
   - Navigate to "Ticket Details"
   - Select ticket ID from dropdown
   - View full ticket information, reasoning, and solution
   - Add manual solution if needed

## 🏗️ Architecture

### Data Structure
```python
Ticket {
    ticket_id: str       # Unique ID (e.g., TKT-0001)
    title: str           # Brief summary
    description: str     # Detailed issue description
    category: str        # Issue category
    status: str          # Open/Resolved
    solution: str        # AI-generated or manual solution
    reasoning: str       # AI reasoning for the solution
    timestamp: str       # Creation date/time
}
```

### RAG Pipeline

1. **Embedding Creation**:
   - Uses HuggingFace `sentence-transformers/all-MiniLM-L6-v2`
   - Creates embeddings from ticket title + description + solution

2. **Vector Storage**:
   - FAISS vector database stores all resolved tickets
   - Enables fast similarity search

3. **Retrieval**:
   - New ticket query searches vector store
   - Returns top 3 most similar past tickets with scores

4. **Generation**:
   - OpenAI GPT-4 analyzes current ticket + similar tickets
   - Generates reasoning and solution
   - Formats response with clear structure

### Storage
- Tickets stored in `tickets.json` file
- Persists across sessions
- Easy to backup and migrate

## 🔧 Configuration

### Change LLM Model
In `app.py`, modify the `generate_solution_with_rag` function:
```python
llm = ChatOpenAI(model="gpt-4", temperature=0.3)
# or use gpt-3.5-turbo for faster/cheaper responses
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0.3)
```

### Adjust Similarity Search
In `find_similar_tickets` function:
```python
# Change number of similar tickets to retrieve
results = vector_store.similarity_search_with_score(query, k=3)  # default: 3
```

### Modify Categories
In the main function's "Submit New Ticket" section:
```python
ticket_category = st.selectbox("Category", 
    ["Hardware", "Software", "Network", "Security", 
     "Access", "Performance", "Other"])  # Add/remove categories
```

## 📊 Example Workflow

1. **First Ticket** (No history):
   - User: "Printer not responding"
   - System: Generates solution based on general knowledge
   - Saves to knowledge base

2. **Second Similar Ticket**:
   - User: "Cannot print documents"
   - System: Finds first ticket as similar
   - Generates solution considering past solution
   - Shows similarity score and reference

3. **Growing Knowledge Base**:
   - More tickets = Better recommendations
   - RAG improves with each resolved ticket
   - Patterns emerge for common issues

## 🎯 Use Cases

- **IT Help Desk**: Automated first-line support
- **Customer Service**: Ticket resolution suggestions
- **Technical Support**: Knowledge base querying
- **Internal IT**: Employee issue tracking
- **Service Management**: ITIL-compliant ticketing

## 🔐 Security Notes

- Keep `.env` file private (contains API keys)
- Don't commit API keys to version control
- Consider encrypting `tickets.json` for sensitive data
- Implement user authentication for production use

## 🚧 Future Enhancements

- [ ] User authentication and role-based access
- [ ] Email notifications for ticket updates
- [ ] Advanced analytics and reporting
- [ ] Multi-language support
- [ ] Integration with external ticketing systems
- [ ] Attachment support for tickets
- [ ] Real-time collaboration features
- [ ] Mobile-responsive design
- [ ] Export tickets to PDF/Excel
- [ ] SLA tracking and escalation

## 📝 License

This is a prototype system for educational/demonstration purposes.

## 🤝 Contributing

Feel free to fork, modify, and enhance this system for your needs!

## 📧 Support

For issues or questions, please refer to the documentation or create an issue.

---

**Built with**: Streamlit, LangChain, OpenAI GPT-4, FAISS, HuggingFace Transformers
