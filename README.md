# 🔐 SignCrypt - AI-Powered Sign Language Communication Platform

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python)
![Flask](https://img.shields.io/badge/Flask-2.3+-green)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.13+-orange)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Gesture%20Detection-red)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Backend Components](#backend-components)
- [Installation Guide](#installation-guide)
- [Configuration](#configuration)
- [API Documentation](#api-documentation)
- [Usage Examples](#usage-examples)
- [Emergency SOS System](#emergency-sos-system)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

**SignCrypt** is an innovative AI-powered platform designed to bridge communication gaps for deaf and hard of hearing individuals. It combines computer vision, machine learning, and real-time gesture recognition to provide a seamless communication experience.

### Mission
Enable inclusive communication through sign language recognition, accessible emergency alerts, and AI-powered learning tools—empowering deaf and hard of hearing communities while supporting hearing individuals in learning ASL (American Sign Language).

---

## ✨ Key Features

### 🤖 **Real-Time Sign Language Recognition**
- **Advanced Computer Vision**: Uses MediaPipe for precise hand landmark detection
- **TensorFlow Lite Models**: Lightweight, optimized models for real-time inference
- **Character Detection**: Recognizes individual sign language letters and special gestures
- **Gesture Stability**: Implements a confidence threshold system to ensure accurate detection
- **Visual Feedback**: Real-time bounding boxes and prediction confidence displays

### 🚨 **Emergency SOS Alert System**
- **Gesture-Based Activation**: Detect emergency gestures (SOS, HELP, EMERGENCY)
- **GPS Integration**: Automatic GPS coordinate collection and sharing
- **SMS Notifications**: Send emergency alerts via Twilio to multiple emergency contacts
- **Customizable Contacts**: Easy management of emergency contact lists
- **Timestamp Tracking**: Log all emergency activations with precise timing

### 💬 **AI Chatbot Integration**
- **Intelligent Conversations**: Chat-based assistance for sign language learning
- **File Upload Support**: Process text and image files for contextual responses
- **User Context Management**: Maintains conversation history and user preferences
- **Accessibility Features**: Designed for non-verbal communication support

### 🔒 **End-to-End Encryption**
- **Message Security**: Fernet-based encryption for sensitive communications
- **User Privacy**: Secure database storage with encrypted sensitive data
- **Safe Data Transmission**: HTTPS-ready infrastructure

### 📚 **Learning Platform**
- **ASL Teaching Modules**: Structured lessons for learning American Sign Language
- **Interactive Training**: Real-time feedback on sign accuracy and practice
- **Progress Tracking**: Monitor learning improvements over time
- **Community Learning**: Share and learn from other users

### 🔄 **Real-Time Communication**
- **WebSocket Integration**: Instant frame processing and response delivery
- **Low Latency**: Optimized for smooth, real-time video streaming
- **Multi-User Support**: Concurrent user sessions with independent state management

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────┐
│                 Frontend (Web/Mobile)                │
│         HTML5 Canvas + WebSocket Client             │
└────────────────────┬────────────────────────────────┘
                     │ WebSocket
                     ▼
┌─────────────────────────────────────────────────────┐
│              Flask Backend (Python)                  │
│                   app.py                             │
├─────────────────────────────────────────────────────┤
│  ┌──────────────────┬──────────────────────────────┐ │
│  │  HTTP Routes     │   WebSocket Events           │ │
│  ├──────────────────┼──────────────────────────────┤ │
│  │ /api/register    │ connect/disconnect           │ │
│  │ /api/encrypt     │ process_frame                │ │
│  │ /api/decrypt     │ clear_text                   │ │
│  │ /api/contacts    │ speak_text                   │ │
│  │ /api/sos         │ toggle_auto_speak            │ │
│  │ /api/chatbot     │ recent_predictions_updated   │ │
│  └──────────────────┴──────────────────────────────┘ │
└────────────┬──────────────────────────┬──────────────┘
             │                          │
             ▼                          ▼
   ┌─────────────────────┐    ┌──────────────────┐
   │ Sign Language Model │    │  SOS Module      │
   │ (TensorFlow Lite)   │    │ & Twilio SMS     │
   │                     │    │                  │
   │ - inference.py      │    │ - sos_module.py  │
   │ - models/*.tflite   │    │ - GPS Tracking   │
   │ - MediaPipe         │    │ - SMS Dispatch   │
   └─────────────────────┘    └──────────────────┘
             │                          │
             └──────────────┬───────────┘
                            │
                    ┌───────▼────────┐
                    │   SQLite DB    │
                    │ (signcrypt.db) │
                    │                │
                    │ - Users        │
                    │ - Contacts     │
                    │ - Activity Log │
                    └────────────────┘
```

---

## 📦 Backend Components

### **1. app.py** - Main Flask Application
The core backend server handling all HTTP routes and WebSocket connections.

#### Key Classes:
- **SignLanguageRecognizer**: Core ML pipeline for gesture recognition
  - Processes video frames in real-time
  - Manages gesture stability detection
  - Handles text composition and special actions

#### Database Models:
```python
User
├── id (Primary Key)
├── name (Unique)
└── created_at (Timestamp)

EmergencyContact
├── id (Primary Key)
├── name
├── phone_number
└── created_at (Timestamp)
```

#### Key Features:
- Real-time frame processing via WebSocket
- User registration and management
- Emergency contact management
- Text encryption/decryption
- SOS triggering with GPS data
- Chatbot integration

---

### **2. sign_language_inference.py** - Gesture Recognition Engine
Handles the ML inference pipeline for sign language detection.

#### Key Functions:
- `predict_with_tflite()`: TensorFlow Lite model inference
- `process_image_for_prediction()`: Image preprocessing and hand detection
- `run_sign_language_inference()`: Webcam-based inference
- `start_realtime_sign_language()`: Initialize real-time recognition
- `process_realtime_frame()`: Process individual video frames

#### Process Flow:
```
Input Frame
    ↓
MediaPipe Hand Detection
    ↓
Extract Hand Landmarks (21 points × 2 coordinates)
    ↓
Normalize Coordinates
    ↓
TensorFlow Lite Inference
    ↓
Get Prediction & Confidence
    ↓
Return Gesture + Annotated Frame
```

---

### **3. sos_module.py** - Emergency Alert System
Comprehensive emergency response system with SMS notifications.

#### Core Classes:

**EmergencyGestureDetector**
- Identifies emergency gestures (SOS, HELP, EMERGENCY, ALERT)
- Confidence threshold validation (>80%)

**GPSLocationService**
- Cross-platform GPS coordinate fetching
- Integrates with frontend for web-based geolocation

**SMSDispatcher**
- Sends emergency SMS via Twilio
- Multi-contact support
- Error handling and retry logic

**SOSMessageGenerator**
- Creates formatted SOS messages
- Includes Google Maps links with GPS coordinates
- Personalized user identification

**EmergencySOSManager**
- Main orchestration class
- Coordinates all SOS subsystems
- Async SOS triggering capability

#### SOS Workflow:
```
Emergency Gesture Detected
    ↓
Collect GPS Coordinates
    ↓
Generate SOS Message with Maps Link
    ↓
Create SMS Dispatcher
    ↓
Send to All Emergency Contacts
    ↓
Log Results & Timestamps
```

---

### **4. chatbot_api.py** - AI Conversation Engine
Provides intelligent chatbot responses for user assistance.

#### Features:
- Context-aware responses
- File processing support
- User session management
- Learning resource recommendations

---

### **5. Additional Utilities**

**collect_imgs.py** - Dataset Collection
- Captures sign images for model training
- Organizes by sign category

**create_dataset.py** - Dataset Preprocessing
- Processes raw images into training format
- Normalizes hand landmarks

**train_classifier.py** - Model Training
- Trains TensorFlow Lite models
- Generates label dictionaries

**inference_tensorflow.py** / **inference_tflite.py** - Model Inference
- Full TensorFlow and lightweight TFLite implementations
- Batch prediction capabilities

---

## 🚀 Installation Guide

### Prerequisites
- Python 3.8 or higher
- pip (Python package manager)
- Webcam (for real-time sign recognition)
- Twilio account (for SOS SMS functionality)

### Step 1: Clone the Repository
```bash
git clone https://github.com/slanguage619-stack/SignCrypt.git
cd SignCrypt/Backend
```

### Step 2: Create Virtual Environment
```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Download Pre-trained Models
```bash
# Create models directory if it doesn't exist
mkdir -p models

# Download or place your TensorFlow Lite model:
# - sign_language_model_simple.tflite
# - tensorflow_labels_dict.pickle

# These should be placed in the Backend/models/ directory
```

### Step 5: Set Up Environment Variables
Create a `.env` file in the Backend directory:

```env
# Flask Configuration
SECRET_KEY=your_strong_random_secret_key_here
ENCRYPTION_KEY=your_fernet_encryption_key

# Twilio Configuration
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_PHONE_NUMBER=your_twilio_phone_number

# Database
DATABASE_URL=sqlite:///signcrypt.db
```

### Step 6: Run the Application
```bash
python app.py
```

The server will start on `http://0.0.0.0:5001`

---

## ⚙️ Configuration

### Twilio SMS Setup

1. **Create Twilio Account**
   - Visit https://www.twilio.com/
   - Sign up for a free trial account

2. **Get Credentials**
   - Find your Account SID and Auth Token in the Dashboard
   - Get a Twilio phone number (e.g., +1-XXXXX-XXXXX)

3. **Set Environment Variables**
   ```bash
   export TWILIO_ACCOUNT_SID="ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   export TWILIO_AUTH_TOKEN="your_auth_token"
   export TWILIO_PHONE_NUMBER="+1XXXXXXXXXX"
   ```

4. **Verify Phone Numbers**
   - For trial accounts, verify recipient phone numbers first
   - Confirmed recipients receive SMS notifications

### Encryption Setup

```python
from cryptography.fernet import Fernet

# Generate a new encryption key (run once)
key = Fernet.generate_key()
print(key.decode())  # Store this as ENCRYPTION_KEY
```

### MediaPipe Configuration

The MediaPipe Hands model is automatically downloaded on first run. Configuration parameters:

- `static_image_mode`: False (for real-time video)
- `min_detection_confidence`: 0.3 (lower = more sensitive)
- `min_tracking_confidence`: 0.3

---

## 📡 API Documentation

### HTTP Routes

#### **Authentication & User Management**

```http
POST /api/register
Content-Type: application/json

{
  "name": "john_doe"
}

Response:
{
  "user": {
    "id": 1,
    "name": "john_doe",
    "created_at": "2025-01-15T10:30:00Z"
  },
  "success": true
}
```

#### **Encryption/Decryption**

```http
POST /api/encrypt
Content-Type: application/json

{
  "message": "Hello, World!"
}

Response:
{
  "encrypted_message": "gAAAAABm...",
  "success": true
}
```

```http
POST /api/decrypt
Content-Type: application/json

{
  "encrypted_message": "gAAAAABm..."
}

Response:
{
  "decrypted_message": "Hello, World!",
  "success": true
}
```

#### **Emergency Contacts Management**

```http
GET /api/contacts

Response:
{
  "contacts": [
    {
      "id": 1,
      "name": "Mom",
      "phone_number": "+1-555-0123",
      "created_at": "2025-01-15T10:30:00Z"
    }
  ]
}
```

```http
POST /api/contacts
Content-Type: application/json

{
  "name": "Mom",
  "phone_number": "+1-555-0123"
}

Response:
{
  "message": "Contact added successfully",
  "contact": { ... }
}
```

```http
DELETE /api/contacts/1

Response:
{
  "message": "Contact deleted successfully"
}
```

#### **SOS Emergency Alert**

```http
POST /api/sos
Content-Type: application/json

{
  "user_name": "John Doe",
  "latitude": 37.7749,
  "longitude": -122.4194
}

Response:
{
  "success": true,
  "message": "SOS triggered successfully",
  "sms_results": {
    "successful": 2,
    "failed": 0,
    "details": [...]
  },
  "contacts_notified": 2
}
```

#### **Chatbot Integration**

```http
POST /api/chatbot
Content-Type: application/json

{
  "message": "How do I sign the letter A?",
  "user_id": "user123",
  "context": {}
}

Response:
{
  "success": true,
  "response": "To sign the letter A, make a fist with your hand...",
  "user_id": "user123",
  "file_processed": false
}
```

### WebSocket Events

#### **Connection & Status**

```javascript
// Client connects
socket.emit('connect')

// Server responds
socket.on('status', (data) => {
  console.log(data.msg)  // "Connected to Sign Language Recognition Server"
})
```

#### **Frame Processing**

```javascript
// Send frame for processing
socket.emit('process_frame', {
  image: 'data:image/jpeg;base64,...'
})

// Receive prediction results
socket.on('prediction_result', (data) => {
  const {
    processed_image,
    prediction_data
  } = data
  
  // prediction_data contains:
  // - predicted_character
  // - stability_count
  // - current_character
  // - displayed_text
  // - hand_detected
})
```

#### **Text Management**

```javascript
// Clear displayed text
socket.emit('clear_text')

socket.on('text_cleared', (data) => {
  console.log('Text cleared:', data.text)
})

// Trigger text-to-speech
socket.emit('speak_text')

socket.on('speak_text', (data) => {
  // Play audio for data.text
})
```

#### **Recent Predictions**

```javascript
// Receive updates on recent predictions
socket.on('recent_predictions_updated', (data) => {
  console.log(data.predictions)
  // Array of recent sign predictions with timestamps
})

// Clear prediction history
socket.emit('clear_recent_predictions')
```

---

## 💻 Usage Examples

### Example 1: Real-Time Sign Language Recognition

```python
from app import app, recognizer
import cv2

def demo_realtime_recognition():
    cap = cv2.VideoCapture(0)
    
    while True:
        ret, frame = cap.read()
        if not ret:
            break
        
        # Process frame through recognizer
        prediction_data, processed_frame = recognizer.process_frame(frame)
        
        # Display results
        print(f"Detected: {prediction_data['predicted_character']}")
        print(f"Stability: {prediction_data['stability_count']}")
        print(f"Text: {prediction_data['displayed_text']}")
        
        cv2.imshow('Sign Language Recognition', processed_frame)
        
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break
    
    cap.release()
    cv2.destroyAllWindows()

if __name__ == '__main__':
    demo_realtime_recognition()
```

### Example 2: Triggering Emergency SOS

```python
from sos_module import EmergencySOSManager, EmergencyContact, GPSCoordinates
from datetime import datetime

# Initialize SOS manager
sos_manager = EmergencySOSManager()

# Set user information
sos_manager.set_user_name("Alex Smith")

# Add emergency contacts
contacts = [
    EmergencyContact("Mom", "+1-555-0100"),
    EmergencyContact("Emergency Services", "+911"),
]
sos_manager.set_emergency_contacts(contacts)

# Trigger SOS with current location
coordinates = GPSCoordinates(
    latitude=37.7749,
    longitude=-122.4194,
    timestamp=datetime.utcnow()
)

result = sos_manager.trigger_sos(coordinates)
print(result)
```

### Example 3: Managing Emergency Contacts via API

```bash
# Add a contact
curl -X POST http://localhost:5001/api/contacts \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Emergency Contact",
    "phone_number": "+1-555-0123"
  }'

# Get all contacts
curl http://localhost:5001/api/contacts

# Delete a contact
curl -X DELETE http://localhost:5001/api/contacts/1
```

### Example 4: Encrypting Sensitive Messages

```bash
# Encrypt a message
curl -X POST http://localhost:5001/api/encrypt \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Sensitive information"
  }'

# Decrypt the message
curl -X POST http://localhost:5001/api/decrypt \
  -H "Content-Type: application/json" \
  -d '{
    "encrypted_message": "gAAAAABm..."
  }'
```

---

## 🚨 Emergency SOS System

### How It Works

1. **Gesture Detection**: User makes emergency gesture (SOS/HELP/EMERGENCY)
2. **Confidence Verification**: System confirms >80% detection confidence
3. **Location Capture**: GPS coordinates automatically fetched
4. **Message Generation**: SOS message created with location map link
5. **SMS Dispatch**: Twilio sends SMS to all emergency contacts
6. **Logging**: All events timestamped and logged

### SOS Message Example

```
🚨 SOS! EMERGENCY 🚨
From: Alex Smith
Location: https://maps.google.com/?q=37.7749,-122.4194
URGENT – Please respond immediately!
```

### Emergency Gesture Recognition

The system recognizes these gestures as emergencies:
- **SOS**: Classic SOS hand signal
- **HELP**: Help gesture in sign language
- **EMERGENCY**: Emergency sign
- **ALERT**: Alert gesture

All require >80% confidence for activation to prevent false alarms.

### SMS Configuration for Different Regions

**Indian Phone Format** (see `INDIAN_PHONE_FORMAT.md`)
```
Supported: +91-XXXXX-XXXXX or 91XXXXXXXXXX
Example: +91-98765-43210
```

---

## 🔧 Troubleshooting

### Issue: Model Not Found

```python
Error: "Error loading TFLite model or labels"
```

**Solution:**
1. Verify model files exist in `Backend/models/`
2. Check file names match:
   - `sign_language_model_simple.tflite`
   - `tensorflow_labels_dict.pickle`

### Issue: Twilio SMS Not Sending

**Checklist:**
1. Verify environment variables are set correctly
2. For trial accounts, ensure phone numbers are verified
3. Check Twilio account balance
4. Review logs for detailed error messages

```bash
# Test Twilio configuration
python debug_sms.py
```

### Issue: MediaPipe Hand Detection Not Working

**Try:**
- Ensure good lighting
- Position hands clearly in frame
- Adjust `min_detection_confidence` (lower = more sensitive)
- Check that `/dev/video0` (Linux) or camera device is accessible

### Issue: WebSocket Connection Fails

**Debug:**
```python
# Check if server is running
curl http://localhost:5001/

# Verify CORS settings
# Check browser console for connection errors
```

---

## 📊 Performance Optimization

### Model Optimization
- Uses TensorFlow Lite for 50x faster inference
- Lightweight model size (~10MB)
- Real-time processing at 30+ FPS

### Database Optimization
- SQLite for local deployment
- Can migrate to PostgreSQL for production
- Indexed queries on frequently accessed fields

### Memory Management
- Circular buffer for recent predictions (max 5)
- Efficient frame preprocessing
- Garbage collection for old sessions

---

## 🛣️ Future Roadmap

- [ ] Full ASL dictionary support (1000+ signs)
- [ ] Computer vision-based pose detection for full body signs
- [ ] Multi-language gesture recognition (ISL, BSL, LSF)
- [ ] Mobile app (iOS/Android) with offline capability
- [ ] Advanced NLP for contextual sign interpretation
- [ ] Classroom learning management system
- [ ] Integration with popular video conferencing platforms
- [ ] Real-time video call translation
- [ ] Machine learning improvements through community data

---

## 📝 Contributing

We welcome contributions from the community! 

### How to Contribute:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Contribution Areas:
- Model training and improvement
- Frontend development
- Bug fixes and optimizations
- Documentation and tutorials
- Testing and QA

---

## 📞 Support & Contact

- **Issues**: Report bugs on GitHub Issues
- **Email**: support@signcrypt.dev
- **Discord**: Join our community server
- **Documentation**: Full docs at [docs.signcrypt.dev](https://docs.signcrypt.dev)

---

## 📄 License

SignCrypt is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **MediaPipe**: Hand tracking and gesture detection
- **TensorFlow**: Machine learning framework
- **Twilio**: SMS messaging platform
- **Flask**: Web framework
- **Community**: All contributors and supporters

---

## 🌍 Mission Statement

**SignCrypt** is committed to fostering inclusive communication for all. By leveraging cutting-edge computer vision and AI, we empower deaf and hard of hearing individuals to communicate effectively, access emergency services quickly, and learn sign language at their own pace. Together, we're breaking down barriers and building bridges.

**"Communication without boundaries. Technology for everyone."**

---

**Last Updated**: January 2025  
**Version**: 1.0.0
