# EPUB Support Guide

## Overview

This localGPT system now fully supports EPUB (Electronic Publication) format books in addition to PDF, DOCX, HTML, TXT, and Markdown files.

## What's New

✅ **EPUB File Processing**: Extract and index content from EPUB books
✅ **Metadata Extraction**: Automatically extracts book title and author information  
✅ **Full-text Search**: Search and query across your entire EPUB book collection
✅ **Web UI Support**: Upload EPUB files through the web interface
✅ **Batch Processing**: Process multiple EPUB files simultaneously

## Installation on Ubuntu

### 1. Install Required Python Libraries

```bash
pip install ebooklib beautifulsoup4
```

Or install all requirements:

```bash
pip install -r requirements.txt
```

### 2. Verify Installation

```python
python -c "import ebooklib; from bs4 import BeautifulSoup; print('✅ EPUB support ready!')"
```

## Usage

### Using the Web Interface

1. **Navigate to** http://localhost:3000
2. **Click** "Create New Index" or attach files to chat
3. **Upload** your .epub files (along with other document types)
4. **Wait** for processing to complete
5. **Start chatting** with your EPUB books!

### Using Command Line

Place your EPUB files in the documents directory and run:

```bash
# Index EPUB books
python -m rag_system.main index --docs-path ./my_ebooks --config-mode standard

# Or use the simple script
./simple_create_index.sh "My Ebooks" "/path/to/ebook.epub"
```

### Programmatic Usage

```python
from rag_system.ingestion.document_converter import DocumentConverter

converter = DocumentConverter()
results = converter.convert_to_markdown("/path/to/book.epub")

for markdown_content, metadata in results:
    print(f"Title: {metadata.get('title')}")
    print(f"Author: {metadata.get('author')}")
    print(f"Content length: {len(markdown_content)} characters")
```

## Supported EPUB Features

✅ **Standard EPUB 2.0 and 3.0 formats**
✅ **Text extraction from all chapters**  
✅ **Metadata (title, author) extraction**
✅ **HTML content parsing**
✅ **Automatic cleanup of scripts and styles**
✅ **Whitespace normalization**

## File Upload Limits

The system accepts these file types:

- `.epub` - EPUB books
- `.pdf` - PDF documents
- `.docx`, `.doc` - Word documents
- `.html`, `.htm` - HTML files
- `.md` - Markdown files
- `.txt` - Plain text files

## Backend Processing

The system processes EPUB files using:

1. **ebooklib** - For reading EPUB structure and content
2. **BeautifulSoup4** - For parsing HTML within EPUB
3. **Document Converter** - For converting to markdown format
4. **Indexing Pipeline** - For embedding and indexing

## Troubleshooting

### EPUB file not recognized

**Problem**: File upload rejected or not processed

**Solution**:

- Ensure file has `.epub` extension
- Verify file is not corrupted: `unzip -t book.epub`
- Check browser console for errors

### Missing dependencies

**Problem**: `ImportError: No module named 'ebooklib'`

**Solution**:

```bash
pip install ebooklib beautifulsoup4
```

### Empty content extracted

**Problem**: EPUB processed but no content found

**Solution**:

- Some EPUBs use non-standard formats
- Check if EPUB is DRM-protected (not supported)
- Verify EPUB opens in other readers (Calibre, etc.)

### Docker deployment

If using Docker, rebuild the images after pulling changes:

```bash
docker-compose down
docker-compose build
docker-compose up -d
```

## Performance Notes

- **EPUB processing** is typically faster than PDF OCR
- **Large EPUB files** (>50MB) may take longer to process
- **Multiple EPUB files** can be batched for parallel processing
- **Memory usage** is proportional to book size

## Example: Indexing an EPUB Library

```bash
# Create directory for your ebooks
mkdir -p ./my_ebooks

# Copy your EPUB files
cp ~/Books/*.epub ./my_ebooks/

# Index all books
python -m rag_system.main index \
  --docs-path ./my_ebooks \
  --config-mode standard \
  --index-name "my_library"

# Start querying
python -m rag_system.main query \
  --index-name "my_library" \
  --query "What are the main themes in the books?"
```

## API Usage

### Upload EPUB via API

```bash
curl -X POST http://localhost:8000/sessions/{session_id}/upload \
  -F "files=@mybook.epub"
```

### Index EPUB Files

```bash
curl -X POST http://localhost:8000/sessions/{session_id}/index
```

## Additional Resources

- **ebooklib Documentation**: https://github.com/aerkalov/ebooklib
- **EPUB Format Spec**: http://idpf.org/epub
- **Calibre (EPUB Manager)**: https://calibre-ebook.com/

## Contributing

Found an EPUB that doesn't work? Please report issues with:

- EPUB file specifications
- Error logs
- Sample file (if not copyrighted)

---

**Note**: DRM-protected EPUB files are not supported. Please ensure you have the right to process the EPUB files you upload.
