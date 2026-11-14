# Voice-to-Image Module Documentation

## Overview

The Voice-to-Image module allows users to generate images using voice input instead of typing. Users can speak their prompt, and the system will convert their speech to text and generate an image.

## Features

- 🎤 **Voice Recording**: Record voice input using browser's Web Speech API
- 📝 **Speech-to-Text**: Real-time transcription of speech to text
- 🖼️ **Image Generation**: Generate images directly from voice prompts
- 🔄 **Dual Input Mode**: Toggle between text and voice input
- 📊 **Analytics Tracking**: Track voice-generated images separately

## How It Works

### Frontend Flow

1. **User clicks "Voice Input" mode** on the Result page
2. **User clicks "Start Voice Input"** button
3. **Browser requests microphone permission** (first time only)
4. **User speaks their prompt** - speech is transcribed in real-time
5. **User clicks "Stop Recording"** when done
6. **Transcribed text appears** in the prompt field
7. **User clicks "Generate Image"** to create the image

### Backend Flow

1. Receives transcript from frontend
2. Validates user credits
3. Calls Clipdrop API with the transcript as prompt
4. Saves image with `source: 'voice'` metadata
5. Logs analytics event `image_generated_from_voice`
6. Creates notification for user

## Browser Compatibility

The Web Speech API is supported in:
- ✅ Chrome (Desktop & Mobile)
- ✅ Edge (Desktop & Mobile)
- ✅ Safari (iOS 14.5+)
- ❌ Firefox (Not supported)
- ❌ Opera (Limited support)

**Note**: For best experience, use Chrome or Edge browsers.

## Usage

### For Users

1. Navigate to the Result page (`/result`)
2. Click the **"🎤 Voice Input"** button
3. Click **"Start Voice Input"** (allow microphone access if prompted)
4. Speak your prompt clearly
5. Click **"Stop Recording"** when finished
6. Review the transcribed text
7. Click **"Generate Image"** to create your image

### For Developers

#### Frontend Component

```jsx
import VoiceRecorder from '../components/VoiceRecorder'

<VoiceRecorder
  onTranscript={(transcript) => {
    // Handle transcript
    setPrompt(transcript)
  }}
  onRecordingChange={(isRecording) => {
    // Handle recording state
    setIsRecording(isRecording)
  }}
/>
```

#### API Endpoint

```javascript
POST /api/voice/generate-from-voice
Headers: { token: "user_token" }
Body: {
  transcript: "A beautiful sunset over mountains"
}
```

Response:
```json
{
  "success": true,
  "message": "Image Generated Successfully from Voice",
  "resultImage": "data:image/png;base64,...",
  "creditBalance": 4,
  "imageId": "image_id",
  "transcript": "A beautiful sunset over mountains"
}
```

## Technical Details

### Speech Recognition

- Uses **Web Speech API** (SpeechRecognition)
- Language: English (en-US)
- Continuous recognition mode
- Interim results enabled for real-time feedback

### Error Handling

- **No speech detected**: Shows info message
- **Microphone denied**: Shows error message
- **Browser not supported**: Shows warning message
- **Network errors**: Handled gracefully with toast notifications

### Analytics

Voice-generated images are tracked separately:
- Event type: `image_generated_from_voice`
- Metadata includes: transcript, generation time, image ID

### Database

Images generated from voice include:
```javascript
{
  metadata: {
    source: 'voice'
  }
}
```

## Troubleshooting

### Microphone Not Working

1. **Check browser permissions**: 
   - Chrome: Settings > Privacy > Site Settings > Microphone
   - Edge: Settings > Privacy > Microphone

2. **Check browser compatibility**: Use Chrome or Edge

3. **Check HTTPS**: Web Speech API requires secure context (HTTPS or localhost)

### Speech Not Being Recognized

1. **Speak clearly** and at a normal pace
2. **Check microphone** is working in other apps
3. **Reduce background noise**
4. **Check internet connection** (some browsers use cloud recognition)

### Transcript Not Accurate

1. **Speak slowly and clearly**
2. **Use common words** (avoid technical jargon)
3. **Check language settings** (currently set to en-US)
4. **Try rephrasing** your prompt

## Future Enhancements

Potential improvements:
- [ ] Multiple language support
- [ ] Server-side speech-to-text (Google Cloud, Azure)
- [ ] Voice commands for image editing
- [ ] Audio file upload support
- [ ] Voice prompt history
- [ ] Real-time image preview while speaking
- [ ] Voice-activated batch generation

## Security & Privacy

- **Client-side processing**: Speech recognition happens in the browser
- **No audio storage**: Audio is not saved or transmitted
- **Only transcript sent**: Only the text transcript is sent to the server
- **Standard API security**: Uses same authentication and rate limiting as text input

## Performance

- **Real-time transcription**: Minimal latency
- **Credit usage**: Same as text input (1 credit per image)
- **Network usage**: Only transcript text sent (minimal bandwidth)

## Support

For issues or questions:
1. Check browser compatibility
2. Verify microphone permissions
3. Try refreshing the page
4. Check console for errors
5. Contact support if issues persist