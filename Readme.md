# Document to Podcast Converter - Turn your DeepResearch into a Podcast :)

Convert Word documents and text files into high-quality MP3 audio using OpenAI's text-to-speech technology.

## Overview

This tool allows you to turn your documents into audio files that you can listen to like podcasts. It supports:

- Word documents (.docx) and text files (.txt)
- Multiple voice options
- Intelligent chunking of large documents
- Caching to avoid regenerating unchanged content
- Batch processing multiple documents in a folder

![image](https://github.com/user-attachments/assets/e8c8c474-3223-4102-9494-bc62adeebfe0)


## Features

- **High-quality TTS**: Uses OpenAI's gpt-4o-mini-tts model for natural-sounding speech
- **Efficient**: Only regenerates audio when document content changes
- **Flexible**: Process individual files or entire directories
- **Customizable**: Choose from multiple voice options
- **Sequential playback**: Automatically plays the generated audio segments in order

## Requirements

- Python 3.6+
- OpenAI API key
- The following Python packages:
  - openai
  - pygame
  - python-docx
  - halo
  - argparse
  - pathlib

## Installation

1. Clone or download this repository
2. Install required packages:
   ```
   pip install openai pygame python-docx halo
   ```
3. Create a `config.json` file in the same directory with the following structure:
   ```json
   {
     "OPENAI_KEY": "your-openai-api-key-here",
     "OUTPUT_DIR": "audio_output",
     "AUDIO_VOICE": "nova"
   }
   ```

## Usage

### Basic Usage

Process a single document:
```
python main.py --document path/to/your/document.docx
```

Process all documents in a folder:
```
python main.py --folder path/to/your/documents
```

### Command Line Options

| Option | Description |
|--------|-------------|
| `--document` | Path to a Word document or text file to convert |
| `--folder` | Path to a folder containing Word documents or text files |
| `--output-dir` | Directory to save the generated audio files (overrides config) |
| `--force` | Force regeneration of audio even if document hasn't changed |
| `--download-only` | Only generate audio files, don't play them back |
| `--voice` | Voice to use (alloy, echo, fable, onyx, nova, shimmer) |
| `--silent` | Don't vocalize actions being performed |
| `--fixed-filename` | Use a specified filename prefix for output files |

### Examples

Convert a document using a specific voice:
```
python main.py --document my_essay.docx --voice shimmer
```

Process all documents in a folder and save to a custom location:
```
python main.py --folder my_documents --output-dir custom_audio
```

Only download audio without playback:
```
python main.py --document report.docx --download-only
```

Force regeneration of audio:
```
python main.py --document presentation.docx --force
```

## How It Works

1. The tool reads the input document(s)
2. It generates a hash of each document to detect changes
3. If the document is unchanged since the last run, it uses the existing audio files
4. For new or changed documents, it:
   - Splits the text into manageable chunks (to meet API limits)
   - Converts each chunk to speech using OpenAI's API
   - Saves the audio files with sequential numbering
5. Finally, it plays back the audio files in sequence (unless `--download-only` is specified)

## Troubleshooting

- **API Key Issues**: Ensure your OpenAI API key is valid and has access to the TTS features
- **File Not Found Errors**: Check paths to ensure documents exist and are accessible
- **Playback Problems**: Ensure pygame is properly installed and your system can play MP3 files
- **Word Document Errors**: Make sure python-docx is installed correctly

## Known Quirks
- App waits for all chunks to download before playback --- for very large docs (10+ pages?) you may want to pass `--download-only` and start playing to mp3s as they arrive. Each will be roughly 4 minutes of audio.

## License
MIT License - do whatever the heck you want to do with it.

## Acknowledgments

- Uses OpenAI's Text-to-Speech API
- Built with pygame for audio playback
- Uses python-docx for Word document processing
