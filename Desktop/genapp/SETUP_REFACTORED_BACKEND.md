# Setup Guide: Refactored Backend

## ✅ What Changed

The backend has been refactored for optimal mobile app integration. Here's what's new:

### Backend Improvements
- ✓ CORS support for mobile apps
- ✓ Environment-based configuration (.env file)
- ✓ Structured logging with request IDs
- ✓ Standardized error responses with error codes
- ✓ Increased timeout (120s) for mobile networks
- ✓ Health check endpoint (`/health`)
- ✓ Better file validation and error messages

### Mobile App Improvements
- ✓ Request ID tracking for debugging
- ✓ Error codes for programmatic handling
- ✓ 120s timeout matches backend
- ✓ Health check support

## 🚀 Quick Start

### 1. Install New Dependencies

```bash
cd LectureToLaTeX
pip install -r requirements.txt
```

This will install:
- `flask-cors` - CORS support
- `python-dotenv` - Environment variable loading

### 2. Create .env File

```bash
cp .env.example .env
```

Then edit `.env` and set your OpenAI API key:

```bash
OPENAI_API_KEY=sk-your-actual-api-key-here
```

### 3. Run the Server

```bash
python app.py
```

You should see:

```
==================================================
LectureToLaTeX Backend Server
==================================================
Host: 0.0.0.0
Port: 8000
Debug: False
Model: gpt-4o
Max file size: 16MB
Timeout: 120s
Docs directory: notes_out
==================================================
```

### 4. Test the Backend

**Test health check:**
```bash
curl http://localhost:8000/health
```

Expected response:
```json
{
  "success": true,
  "data": {
    "status": "healthy",
    "version": "1.0.0",
    "model": "gpt-4o",
    "timestamp": "2025-12-03T..."
  }
}
```

**Test chat endpoint:**
```bash
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -H "X-Request-ID: test-123" \
  -d '{"message": "2+2", "use_llm": false}'
```

Expected response:
```json
{
  "success": true,
  "data": {
    "reply": "$$4$$",
    "used_llm": false
  }
}
```

## 📱 Mobile App Setup

The mobile app (`LectureNotesApp/services/api.js`) has been updated to:
- Send request IDs with every API call
- Use 120s timeout
- Parse new error response format
- Call `/health` endpoint for connectivity checks

No changes needed in your mobile app code!

## 🐛 Debugging

### View Logs

Logs are now written to `app.log`:

```bash
tail -f app.log
```

Every request has a unique ID:

```
2025-12-03 15:30:45 [INFO] __main__: [abc123] POST /upload
2025-12-03 15:30:45 [INFO] __main__: [abc123] Saved file 1/1: photo_0.jpg (1234567 bytes)
2025-12-03 15:30:45 [INFO] __main__: [abc123] Processing 1 images...
```

### Error Responses

All errors now include:
- `error.code` - Programmatic error code (e.g., `FILE_TOO_LARGE`)
- `error.message` - Human-readable message
- `error.request_id` - For tracking in logs

Example error:
```json
{
  "success": false,
  "error": {
    "code": "FILE_TOO_LARGE",
    "message": "File photo.jpg exceeds 16MB limit",
    "request_id": "mobile-1234567890"
  }
}
```

### Common Issues

**Import errors (dotenv, flask_cors):**
```bash
pip install flask-cors python-dotenv
```

**API key error:**
```
ValueError: OPENAI_API_KEY must be set in environment or .env file
```
Solution: Create `.env` file with your API key

**Port 8000 in use:**
```bash
# Change port in .env
PORT=8001
```

## 🔧 Configuration Options

Edit `.env` to customize:

| Variable | Default | Description |
|----------|---------|-------------|
| `OPENAI_API_KEY` | required | Your OpenAI API key |
| `MODEL_NAME` | gpt-4o | Model to use for OCR |
| `PORT` | 8000 | Server port |
| `HOST` | 0.0.0.0 | Server host (use 0.0.0.0 for mobile) |
| `MAX_FILE_SIZE_MB` | 16 | Max upload size |
| `TIMEOUT_SECONDS` | 120 | Request timeout |
| `FLASK_DEBUG` | false | Enable debug mode |
| `DOCS_DIR` | notes_out | LaTeX output directory |

## 📊 Testing with Mobile App

1. **Start backend** (as shown above)

2. **Update mobile app config:**

   Edit `LectureNotesApp/utils/config.js`:
   ```javascript
   export const API_CONFIG = {
     BASE_URL: 'http://YOUR_IP:8000',  // Your computer's IP
   };
   ```

3. **Run mobile app:**
   ```bash
   cd LectureNotesApp
   npm start
   ```

4. **Test features:**
   - Try taking a photo → Should upload and process
   - Try chatbot → Should get math responses
   - Check error handling → Try invalid inputs

5. **View request IDs:**
   - Mobile app sends `X-Request-ID: mobile-1234...`
   - Backend logs show `[mobile-1234...] Processing...`
   - Errors include same request ID for debugging

## 🎯 What to Test

- [ ] Backend starts without errors
- [ ] `/health` endpoint responds
- [ ] Chat endpoint works with test message
- [ ] Mobile app can upload photos
- [ ] Mobile app chatbot works
- [ ] Error messages are clear
- [ ] Logs show request IDs
- [ ] 120s timeout allows processing to complete

## 📝 Notes

- The backend now logs to both console and `app.log`
- Request IDs help correlate mobile errors with backend logs
- All endpoints return consistent `{success, data}` or `{success, error}` format
- CORS is enabled for development (restrict in production)
- Timeouts increased from 60s to 120s for mobile networks

## 🆘 Getting Help

If you encounter issues:

1. Check `app.log` for errors
2. Look for request ID in mobile app error
3. Search logs for that request ID
4. Check BACKEND_TROUBLESHOOTING.md for solutions
