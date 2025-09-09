# Dictationist Agentic AI for Tamil YouTube Videos

This project is an **end-to-end AI pipeline** for processing YouTube videos into **knowledge-enhanced Tamil & English subtitles** and enabling **RAG-based Q&A**.  
It integrates **audio transcription, metadata scraping, Named Entity Recognition (NER), knowledge base construction, and agentic subtitle refinement** into a single automated system.

---

## Features

✅ **Audio Extraction** from YouTube via `yt-dlp` + `ffmpeg`  
✅ **Speech-to-Text Transcription** using [WhisperX](https://github.com/m-bain/whisperx) with **speaker diarization** & **Tamil alignment**  
✅ **Agentic AI Subtitle Processor**:
- Generates **polished Tamil subtitles**  
- Translates into **natural English subtitles**  
- Uses **Knowledge Base (KB) + Error Memory** for iterative refinement  
✅ **YouTube Metadata & Web Context Scraping** for **Named Entities (NER)**  
✅ **Persistent ChromaDB Knowledge Base** with multilingual embeddings  
✅ **Retrieval-Augmented Generation (RAG) Q&A** in **Tamil & English**  
✅ **Ollama Integration** for LLM-powered reasoning with **Gemma 12B**  

flowchart TD
    A[YouTube URL] --> B[Audio Extraction (yt-dlp + ffmpeg)]
    B --> C[Transcription (WhisperX, Tamil aligned)]
    A --> D[Metadata + Page Scraping]
    D --> E[LLM NER Extraction]
    E --> F[Entity Web Search + Context]
    C --> G[Agentic Tamil Subtitle Agent]
    G --> H[English Subtitle Translation]
    G & H --> I[Error Memory KB]
    D & E & F --> J[ChromaDB Knowledge Base]
    J --> K[RAG Q&A Function]
    I --> G
```

---

## Installation

### 1. Clone Repository
```bash
git clone https://github.com/yourusername/yt-agentic-subtitles.git
cd yt-agentic-subtitles
```

### 2. Install Dependencies
Run the setup cell in **Colab** or install locally:
```bash
pip install -q yt-dlp ffmpeg-python sentence-transformers chromadb langchain-text-splitters duckduckgo-search beautifulsoup4 readability-lxml tldextract httpx ollama
apt-get install -y ffmpeg libcudnn8 pciutils
```

### 3. Install WhisperX
```bash
pip install git+https://github.com/m-bain/whisperx.git
```

### 4. Install & Start Ollama
```bash
curl -fsSL https://ollama.com/install.sh | sh
nohup ollama serve > ollama.log 2>&1 &
```

Pull the model:
```bash
ollama pull gemma3:12b
```

---

## Usage

### 1. Run Full Pipeline
```python
from main import process_youtube_video

youtube_url = "https://www.youtube.com/watch?v=Cz_9yG2FQ3E"
tamil_srt, english_srt = process_youtube_video(youtube_url)
print("Tamil Subtitles saved at:", tamil_srt)
print("English Subtitles saved at:", english_srt)
```

This performs:
1. Extracts audio from YouTube  
2. Transcribes using WhisperX  
3. Scrapes metadata + performs NER  
4. Runs **Agentic AI** for Tamil + English subtitles  

### 2. Ask Questions (RAG Q&A)
```python
from main import rag_query

print(rag_query("Summarize the video and list key people", language="english"))
print(rag_query("இந்த வீடியோவின் முக்கிய நபர்களை சொல்லுங்கள்", language="tamil"))
```

---

## Project Structure

```
├── audio_data/          # Extracted YouTube audio
├── whisperx_data/       # WhisperX transcripts (3s, 6s, 15s, 30s chunks)
├── kb_data/             # ChromaDB persistent KB
├── llm_data/            # Final Tamil & English subtitles
├── main.py              # Main pipeline script
├── README.md            # Documentation
```

---

## How the Agentic AI Works

1. **Multiple Transcripts (3s, 6s, 15s, 30s)** → merged for context  
2. **Tamil Subtitle Agent**:
   - Uses **base timestamps from 6s transcript**  
   - Iteratively refines with **Knowledge Base** + **Error Memory**  
   - Validates against SRT formatting rules  
3. **English Subtitle Agent**:
   - Translates Tamil SRT into natural English  
   - Preserves timestamps & entity consistency  
4. **Error Memory KB**:
   - Stores past subtitle issues  
   - Ensures system **learns from mistakes** across runs  

---

## Requirements

- Python 3.9+  
- CUDA-enabled GPU (for WhisperX with `large-v2`)  
- Hugging Face token (for diarization model: `Amrrs/wav2vec2-large-xlsr-53-tamil`)  

Store HF token in Colab with:
```python
from google.colab import userdata
userdata.set("HF_TOKEN", "your_huggingface_token")
```

---

## Example Output

- `Tamil_subtitles.srt` → Polished Tamil subtitles  
- `English_subtitles.srt` → English translations  
- KB entries: YouTube metadata, NER, web snippets  
- RAG answers: Context-aware Q&A  

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
