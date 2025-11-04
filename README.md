# Music Recommendation Agent 🎵

An intelligent agentic web application that recommends music based on your current mood, activity, location, and time of day. Built with Python, Flask, and Google's Gemini AI.

## Features

- 🎯 **Context-Aware Recommendations**: Considers mood, activity, time, and location
- 🧠 **Agentic Architecture**: Modular design with perception, memory, decision, and action layers
- 🔧 **MCP Tools**: Each action is an MCP (Model Context Protocol) tool
- 🤖 **Gemini AI Orchestration**: Uses Gemini 1.5 Flash for intelligent decision making
- 💾 **Memory System**: Stores interactions and learns from context
- ⚙️ **Configurable System Prompt**: Easily modify agent behavior
- 🌐 **Beautiful Web Interface**: Modern, intuitive UI with geolocation

## Architecture

The application follows a layered agentic architecture:

```
┌─────────────────────────────────────────┐
│         Web Interface (Flask)           │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│    1. Perception Layer (perception.py)  │
│    - Processes user inputs              │
│    - Extracts semantic tags             │
│    - Structures context                 │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│    2. Decision Layer (decisions.py)     │
│    - Gemini 2.0-flash orchestration     │
│    - Chooses which MCP tools to call    │
│    - Configurable system prompt         │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│    3. Action Layer (actions.py)         │
│    - MCP Tool: recommend_music          │
│    - Matches music to context           │
│    - Returns recommendations            │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│    4. Memory Layer (memory.py)          │
│    - Stores interactions                │
│    - Maintains conversation context     │
│    - Tracks importance scores           │
└─────────────────────────────────────────┘
```

## Installation

1. **Clone or download this repository**

2. **Install dependencies using uv**:
   ```bash
   uv sync
   ```

3. **Set up environment variables**:
   Create a `.env` file or export the following variable:
   ```bash
   export GEMINI_API_KEY="your_api_key_here"
   ```
   
   Get your API key from: https://aistudio.google.com/apikey

## Usage

### Running the Application

1. **Start the web server**:
   ```bash
   uv run python main.py
   ```

2. **Open your browser** and navigate to:
   ```
   http://localhost:5000
   ```

3. **Enter your context**:
   - Describe your current **mood** (e.g., happy, calm, energetic)
   - Describe what you're **doing** (e.g., working, exercising, relaxing)
   - Location and time are automatically detected

4. **Get recommendations**: Click "Get Music Recommendations" and enjoy!

### Example Usage

- **Mood**: "happy rised energetic"
- **Activity**: "exercising at the gym"
- **Result**: High-energy workout songs

- **Mood**: "calm and peaceful"
- **Activity**: "working on coding project"
- **Result**: Ambient focus music

## Configuration

### System Prompt

Modify the agent's behavior by updating the system prompt:

```bash
# Check current prompt
curl http://localhost:5000/config/system-prompt

# Update prompt
curl -X POST http://localhost:5000/config/system-prompt \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Your custom prompt here"}'
```

Or modify directly in code at `decisions.py`:

```python
SystemPromptConfig.update_prompt("Your custom prompt")
```

### Memory System

View memory statistics:

```bash
curl http://localhost:5000/memory/stats
```

Get recent memories:

```bash
curl http://localhost:5000/memory/recent?limit=5
```

## API Endpoints

- `GET /` - Web interface
- `POST /recommend` - Get music recommendations
- `GET /config/system-prompt` - Get current system prompt
- `POST /config/system-prompt` - Update system prompt
- `GET /memory/stats` - Get memory statistics
- `GET /memory/recent` - Get recent memories

## Project Structure

```
agentic-app/
├── main.py              # Main Flask application
├── perception.py        # Perception layer
├── decisions.py         # Decision layer with Gemini
├── actions.py           # Action layer with MCP tools
├── memory.py            # Memory layer
├── templates/
│   └── index.html       # Web interface
├── pyproject.toml       # Dependencies
└── README.md           # This file
```

## How It Works

1. **User Input**: User provides mood and activity through the web interface
2. **Perception**: System captures inputs, time, and geolocation
3. **Decision**: Gemini AI analyzes context and decides which tool to use
4. **Action**: MCP tool searches music database and matches songs
5. **Memory**: Interaction is stored for future context
6. **Response**: Formatted recommendations are returned to the user

## MCP Tools

### recommend_music

Recommends music based on mood, activity, and context.

**Parameters**:
- `mood` (string): User's current mood
- `activity` (string): User's current activity
- `tags` (array, optional): Semantic tags

**Returns**:
- List of music recommendations with song, artist, genre, and reasoning

## Extending the System

### Adding New Tools

1. Create tool definition in `actions.py`:
```python
class MyNewTool:
    def get_tool_definition(self):
        return {...}
    
    def execute_tool(self, arguments):
        return {...}
```

2. Register in `main.py`:
```python
decision_maker.register_tool("my_tool", tool.get_tool_definition())
```

3. Update decision logic in `decisions.py`

### Customizing Music Database

Edit the `MUSIC_LIBRARY` in `actions.py` to add or modify songs:

```python
MUSIC_LIBRARY = [
    {
        "song": "Song Title",
        "artist": "Artist Name",
        "genre": "Genre",
        "energy": "high|medium|low",
        "mood_tags": ["tag1", "tag2"],
        "activity_tags": ["tag1", "tag2"]
    },
    ...
]
```

## Troubleshooting

### "GEMINI_API_KEY environment variable must be set"

The app can run without the API key in fallback mode, but you'll get better results with it. Set it using:

```bash
export GEMINI_API_KEY="your_key_here"
```

### Location not detected

This is optional. The app will work fine without location data.

### Port already in use

Change the port in `main.py`:

```python
app.run(debug=True, host='0.0.0.0', port=8080)  # Use port 8080
```

## License

MIT License - feel free to use and modify as needed!

## Contributing

Contributions are welcome! Feel free to:
- Add more music to the database
- Implement new MCP tools
- Improve the web interface
- Enhance the memory system

## Acknowledgments

- Built with Flask, Pydantic, and Google Gemini AI
- Uses MCP (Model Context Protocol) for tool definitions
- Inspired by agentic AI architectures

# agentic-app
