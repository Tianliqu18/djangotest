# LectureToLaTeX Mobile App

A React Native app that converts lecture photos to LaTeX notes using OCR. Take multiple photos of blackboards or whiteboards and automatically generate LaTeX documents.

## Features

- 📸 **Multi-Photo Capture**: Take multiple photos in one session
- 📄 **LaTeX Conversion**: Automatic OCR and LaTeX generation via backend
- 💾 **Local Storage**: Documents saved locally with AsyncStorage
- 🎨 **Clean UI**: Matches Figma design mockups
- ⚡ **Real-time Processing**: Shows progress while converting

## Prerequisites

- Node.js (v14+)
- npm or yarn
- Expo Go app (for testing on physical device)
- **LectureToLaTeX backend running** (https://github.com/Kai-Gowers/LectureToLaTeX)

## Setup

### 1. Install Dependencies

```bash
npm install
```

### 2. Configure Backend URL

Edit `utils/config.js` to point to your backend:

```javascript
export const API_CONFIG = {
  BASE_URL: 'http://YOUR_IP_ADDRESS:5000',  // Change this!
};
```

**Important**: 
- For iOS simulator: Use `http://localhost:5000`
- For Android emulator: Use `http://10.0.2.2:5000`
- For physical device: Use your computer's IP address (e.g., `http://192.168.1.100:5000`)

### 3. Start Backend

Make sure the LectureToLaTeX backend is running:

```bash
# In the backend directory
export OPENAI_API_KEY="your-api-key"
python app.py
```

The backend should be running on port 5000.

### 4. Run the App

```bash
npm start
```

Then:
- Scan QR code with Expo Go app
- Press 'i' for iOS simulator
- Press 'a' for Android emulator
- Press 'w' for web (limited functionality)

## Usage

### Creating a Document

1. **Tap the + button** in the bottom tab bar
2. **Enter a document name** in the modal
3. **Take photos** of lecture notes/blackboards
   - Tap the large white button to capture
   - Photos appear as thumbnails at the top
   - Take as many photos as needed
4. **Tap the green checkmark** when done
5. **Wait for processing** - the app will extract text and generate LaTeX
6. **View your document** in the Files tab

### Managing Documents

- **View all documents**: Files tab shows all saved documents
- **Delete a document**: Long-press on any document card
- **Refresh list**: Pull down to refresh

## Project Structure

```
LectureNotesApp/
├── App.js                    # Navigation setup
├── screens/
│   ├── FilesScreen.js       # Document list
│   └── CameraScreen.js      # Photo capture + upload
├── services/
│   ├── api.js               # Backend communication
│   └── storage.js           # AsyncStorage wrapper
├── utils/
│   └── config.js            # Backend URL configuration
└── constants/
    └── colors.js            # Design system
```

## Troubleshooting

### "Upload failed" Error

1. **Check backend is running**: Visit http://localhost:5000 in browser
2. **Check IP address**: Make sure `utils/config.js` has correct URL
3. **Check firewall**: Ensure port 5000 is not blocked
4. **Check API key**: Backend needs OPENAI_API_KEY environment variable

### Camera Permission Denied

- iOS: Go to Settings → Privacy → Camera → Expo Go → Enable
- Android: Go to Settings → Apps → Expo Go → Permissions → Enable Camera

### "No documents yet"

This is normal for first run. Tap the + button to create your first document.

## Backend Configuration

The app expects the LectureToLaTeX backend with these endpoints:

- `POST /upload` - Upload photos for OCR/LaTeX conversion
- `GET /download/<note_name>?type=tex` - Download LaTeX file

See https://github.com/Kai-Gowers/LectureToLaTeX for backend setup.

## Development Notes

- Photos are compressed before upload for better performance
- Documents stored locally survive app restarts
- Backend connection tested before upload
- Comprehensive error handling with user-friendly messages

## Future Enhancements

- PDF preview rendering
- LaTeX editing in-app
- Cloud sync
- Export/share functionality
- Batch operations
- Document search

## License

Created for educational purposes.
