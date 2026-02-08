# 🍷 AI Sommelier

An expert AI wine recommendation agent built with OpenAI's Assistants API v2 and file_search capability. Provides personalized wine pairings, recommendations, and education grounded in a wine catalog PDF.

## ✨ Features

- **🎯 Personalized Recommendations**: Tailored to your food, taste preferences, budget, and occasion
- **📚 PDF-Grounded Responses**: All wine facts retrieved from a real catalog (no hallucinations)
- **💬 Stateful Conversations**: Multi-turn dialogue with context awareness
- **🍽️ Classic Pairing Principles**: Matches intensity, balances acidity, fat, spice, sweetness, and tannin
- **🔍 Citation Support**: References specific passages from the wine catalog

## 🚀 Quick Start

### Prerequisites

1. **OpenAI API Key**: Get one from [platform.openai.com](https://platform.openai.com/api-keys)
2. **Google Colab Account**: Free access at [colab.research.google.com](https://colab.research.google.com)
3. **Wine Catalog PDF**: `Vinhos baba d_urso.pdf` (or your own wine catalog)

### Setup Instructions

1. **Open the Notebook**
   - Upload `AI_Sommelier.ipynb` to Google Colab
   - Or open directly from GitHub

2. **Configure API Key**
   - Click the key icon 🔑 in Colab's left sidebar
   - Add a new secret:
     - Name: `OPENAI_API_KEY`
     - Value: Your OpenAI API key

3. **Upload Wine PDF**
   - Drag and drop `Vinhos baba d_urso.pdf` into Colab's Files panel
   - Or mount Google Drive and update the path

4. **Run Setup Cells**
   - Execute cells 1-4 in order
   - Copy the vector store ID when prompted
   - Paste it back into cell 3 for future sessions

5. **Start Chatting**
   - Run cell 6 to begin the interactive chat
   - Ask for wine recommendations!

## 📖 Usage Examples

### Basic Pairing Request
```
You: Suggest a wine for grilled salmon with lemon, budget under $30
Sommelier: [Provides 2-4 wine recommendations with details]
```

### Detailed Preferences
```
You: I want a bold red for ribeye steak, no sweet wines, prefer something from Portugal
Sommelier: [Filtered recommendations matching all criteria]
```

### Spicy Food Pairing
```
You: Pair a wine with spicy Thai curry
Sommelier: [Recommends wines with acidity and aromatics]
```

### Wine Education
```
You: Tell me about wines from the Douro region
Sommelier: [Educational response grounded in PDF content]
```

## 🏗️ Architecture

### Components

1. **OpenAI Assistants API v2**: Powers the conversational agent
2. **File Search Tool**: Retrieves relevant information from the wine PDF
3. **Vector Store**: Indexes and stores the wine catalog for semantic search
4. **Session Management**: Maintains conversation context across messages

### Workflow

```
User Query → Thread Message → Assistant Run → File Search → 
Retrieval from PDF → Response Generation → Citation Formatting → Display
```

### Key Design Decisions

- **Single Agent Architecture**: Simpler, faster, more cost-effective than multi-agent handoffs
- **One-Time Vector Store**: Create once, reuse to avoid re-indexing costs
- **Colab Secrets**: Secure API key storage without exposing credentials
- **Citation Extraction**: Displays PDF sources to ensure transparency

## 💰 Cost Estimates

| Component | Cost | Frequency |
|-----------|------|-----------|
| Vector Store Storage | ~$0.10/GB/day | Daily (minimal for small PDFs) |
| File Search Queries | ~$0.03/GB | Per query (pennies) |
| GPT-4o Model | ~$0.01-0.03 | Per query |
| **Typical Session (10 queries)** | **~$0.20-0.40** | **Per session** |

**Cost Optimization Tips:**
- Reuse `VECTOR_STORE_ID` instead of recreating
- Use smaller, focused PDFs (< 10MB)
- Delete unused vector stores from OpenAI dashboard

## 📁 Project Structure

```
AI-Sommelier/
├── AI_Sommelier.ipynb       # Main Colab notebook
├── prompt.txt                # Agent instructions and behavior rules
├── README.md                 # This file
└── Vinhos baba d_urso.pdf   # Wine catalog (not included)
```

## 🛠️ Technical Details

### Dependencies

```python
openai>=1.0.0          # OpenAI SDK with Assistants API v2
python-dotenv          # Environment variable management
```

### Agent Instructions

The sommelier follows strict rules defined in `prompt.txt`:

- **Retrieval First**: Always searches the PDF before responding
- **No Hallucinations**: Only recommends wines present in the catalog
- **Budget Enforcement**: Strictly respects price constraints
- **Structured Output**: Consistent format for all recommendations
- **Fallback Handling**: Asks clarifying questions when info is missing

### Recommendation Format

Each wine recommendation includes:
- Wine name (exactly from PDF)
- Country/region
- Grape variety or blend
- Style profile (body, acidity, tannin, sweetness)
- Pairing rationale
- Serving temperature
- Optional: Decanting/glassware notes

## 🔧 Troubleshooting

### API Key Error
**Problem**: "Could not retrieve OPENAI_API_KEY"
**Solution**: Add API key to Colab Secrets (🔑 icon), ensure it's named exactly `OPENAI_API_KEY`

### PDF Not Found
**Problem**: "Wine catalog PDF not found"
**Solution**: 
- Upload PDF to Colab Files panel
- Or mount Google Drive: `from google.colab import drive; drive.mount('/content/drive')`
- Update `WINE_PDF_PATH` in cell 3

### Vector Store Error
**Problem**: "Error retrieving vector store"
**Solution**: Set `VECTOR_STORE_ID = None` in cell 3 and re-run to create fresh vector store

### No Recommendations Found
**Problem**: Agent says "wine not found in catalog"
**Solution**: 
- Verify PDF contains relevant wines
- Try broader queries (agent can't invent wines)
- Check if PDF was properly indexed (run utility: `check_vector_store_status()`)

### Timeout Errors
**Problem**: "Request timed out"
**Solution**: Large PDFs may take longer - increase timeout in `send_message()` function

## 🔄 Updating Wine Catalog

To use a new/updated wine PDF:

1. Set `VECTOR_STORE_ID = None` in cell 3
2. Update `WINE_PDF_PATH` with new file location
3. Re-run cell 3 to create new vector store
4. Save the new vector store ID for future use

**Note**: Old vector stores will continue to incur storage costs until deleted from [OpenAI dashboard](https://platform.openai.com/storage).

## 🧪 Testing

The notebook includes automated testing (cell 7):
- Grilled fish pairing
- Red wine for steak
- Spicy food pairing
- Regional wine queries
- Budget-constrained recommendations

Run these tests after setup to verify everything works correctly.

## 📚 Additional Resources

- [OpenAI Assistants API Documentation](https://platform.openai.com/docs/assistants/overview)
- [File Search Tool Guide](https://platform.openai.com/docs/assistants/tools/file-search)
- [Vector Stores Documentation](https://platform.openai.com/docs/assistants/tools/file-search/vector-stores)

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Multi-agent handoffs for specialized tasks (education, tasting notes)
- Gradio/Streamlit web interface
- Multi-language support
- Enhanced citation formatting
- Wine image integration
- Persistent storage (database integration)

## 📄 License

This project is provided as-is for educational and personal use.

## 🙏 Acknowledgments

Built with:
- [OpenAI Assistants API](https://platform.openai.com/docs/assistants)
- [Google Colab](https://colab.research.google.com)
- Wine catalog: Vinhos Baba d'Urso

---

**Enjoy your wine journey! 🍷🥂**

*For questions or issues, please open an issue on GitHub.*
