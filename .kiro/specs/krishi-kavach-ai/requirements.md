# Requirements Document: Krishi-Kavach AI

## Introduction

Krishi-Kavach AI is an offline-first edge vision and multimodal agentic advisor designed for rural Indian farmers with limited or intermittent connectivity. The system employs a hybrid-compute architecture with two tiers: Tier 1 (Edge/Offline) for instant disease detection using WebAssembly and TensorFlow Lite, and Tier 2 (Cloud/Agentic) for detailed advisory using Gemini 1.5 Flash and Bhashini multilingual support. This enables farmers to receive immediate diagnostic results offline while accessing comprehensive guidance when connectivity is available.

## Glossary

- **System**: The complete Krishi-Kavach AI application
- **Edge_Module**: The offline WebAssembly-based computer vision component running on device
- **Agent_Module**: The cloud-based multimodal AI advisor using Gemini 1.5 Flash
- **Digital_Vaidya**: The offline disease detection feature for livestock and crops
- **Bhasha_Kisan**: The multilingual AI agent providing agricultural advisory
- **Bhashini_Service**: The government multilingual translation service
- **Detection_Result**: Output from computer vision analysis containing disease/pest identification
- **Advisory_Response**: Detailed recommendations from the AI agent
- **User**: Farmer or agricultural extension worker using the system
- **Connectivity_State**: Network availability status (offline, 2G, 3G, 4G, etc.)
- **Wasm_Runtime**: WebAssembly execution environment in the browser/app
- **TFLite_Model**: TensorFlow Lite model for edge inference
- **Lumpy_Skin_Disease**: Viral disease affecting cattle and buffalo
- **Pest_Detection**: Identification of crop-damaging insects and diseases
- **Sub_Dialect**: Regional language variation specific to user's locality
- **Multimodal_Input**: Image, video, or voice input from user

## Requirements

### Requirement 1: Offline Disease Detection

**User Story:** As a farmer in a remote area without internet connectivity, I want to detect livestock and crop diseases instantly using my phone's camera, so that I can take immediate action without waiting for network access.

#### Acceptance Criteria

1. WHEN a user captures an image of livestock or crops, THE Edge_Module SHALL process the image locally within 3 seconds
2. WHEN processing images offline, THE Edge_Module SHALL use TFLite_Model running in Wasm_Runtime without requiring network connectivity
3. WHEN Lumpy_Skin_Disease is detected in livestock images, THE Edge_Module SHALL return a Detection_Result with confidence score above 0.75
4. WHEN crop pest detection is performed, THE Edge_Module SHALL identify common pests with minimum 80% accuracy
5. WHEN the device is offline, THE System SHALL display Detection_Result immediately without attempting network calls
6. WHEN multiple images are captured in sequence, THE Edge_Module SHALL process each image independently and maintain performance

### Requirement 2: WebAssembly Edge Computing

**User Story:** As a system architect, I want the computer vision models to run efficiently in WebAssembly on mobile devices, so that farmers can use the app on low-end smartphones without performance issues.

#### Acceptance Criteria

1. THE Edge_Module SHALL compile TFLite_Model to WebAssembly format for browser execution
2. WHEN the app loads, THE Wasm_Runtime SHALL initialize within 5 seconds on devices with 2GB RAM
3. WHEN performing inference, THE Edge_Module SHALL use less than 100MB of device memory
4. THE Edge_Module SHALL support both Android and iOS browsers through WebAssembly
5. WHEN the device battery is below 20%, THE Edge_Module SHALL continue functioning without degradation
6. THE System SHALL cache Wasm_Runtime and TFLite_Model locally for offline availability

### Requirement 3: Hybrid Connectivity Management

**User Story:** As a farmer with intermittent network access, I want the app to automatically use cloud features when I have connectivity and work offline otherwise, so that I can always use the core detection features regardless of network availability.

#### Acceptance Criteria

1. WHEN the app starts, THE System SHALL detect Connectivity_State and configure available features accordingly
2. WHEN Connectivity_State changes from offline to online, THE System SHALL automatically enable Agent_Module features
3. WHEN Connectivity_State is offline, THE System SHALL disable cloud-dependent features and show only Edge_Module capabilities
4. WHEN network quality is 2G, THE System SHALL optimize data transfer to use less than 500KB per advisory request
5. WHILE Connectivity_State is unstable, THE System SHALL queue user requests and process them when connection stabilizes
6. THE System SHALL persist Detection_Result locally and sync to cloud when Connectivity_State becomes available

### Requirement 4: Multimodal AI Agent Advisory

**User Story:** As a farmer who has detected a disease, I want to send images, videos, or voice descriptions to get detailed treatment recommendations in my local language, so that I can understand and implement the solutions effectively.

#### Acceptance Criteria

1. WHEN Connectivity_State is available, THE Agent_Module SHALL accept Multimodal_Input including images, videos up to 30 seconds, and voice recordings
2. WHEN processing Multimodal_Input, THE Agent_Module SHALL use Gemini 1.5 Flash for analysis and generate Advisory_Response within 10 seconds
3. WHEN video input is provided, THE Agent_Module SHALL analyze key frames and audio to provide comprehensive disease assessment
4. WHEN voice input is received, THE Agent_Module SHALL transcribe speech and extract agricultural context for advisory generation
5. THE Agent_Module SHALL combine Edge_Module Detection_Result with Multimodal_Input for enhanced accuracy
6. WHEN generating Advisory_Response, THE Agent_Module SHALL include treatment steps, preventive measures, and resource recommendations

### Requirement 5: Multilingual Support with Bhashini

**User Story:** As a farmer who speaks a regional dialect, I want to interact with the system in my native language and receive responses in my specific sub-dialect, so that I can fully understand the agricultural advice without language barriers.

#### Acceptance Criteria

1. THE System SHALL integrate with Bhashini_Service to support 22 Indian languages
2. WHEN a user selects their language preference, THE System SHALL configure Bhashini_Service for that Sub_Dialect
3. WHEN generating Advisory_Response, THE Agent_Module SHALL translate content using Bhashini_Service to match user's Sub_Dialect
4. WHEN voice input is provided, THE System SHALL use Bhashini_Service for speech-to-text in the user's language
5. WHEN delivering Advisory_Response, THE System SHALL provide text-to-speech output in the user's Sub_Dialect
6. THE System SHALL cache common translations locally for offline access to previously received advice

### Requirement 6: Lumpy Skin Disease Detection

**User Story:** As a livestock owner, I want to quickly identify if my cattle have Lumpy Skin Disease by taking photos of visible symptoms, so that I can isolate affected animals and seek treatment immediately.

#### Acceptance Criteria

1. WHEN an image of cattle skin is captured, THE Edge_Module SHALL analyze for Lumpy_Skin_Disease characteristic nodules and lesions
2. WHEN Lumpy_Skin_Disease is detected, THE Detection_Result SHALL include severity level (mild, moderate, severe)
3. WHEN confidence score is below 0.75, THE Detection_Result SHALL indicate uncertainty and recommend additional images
4. THE Edge_Module SHALL detect Lumpy_Skin_Disease across different cattle breeds and lighting conditions
5. WHEN multiple lesions are present, THE Detection_Result SHALL count and locate affected areas
6. THE System SHALL provide immediate offline guidance for isolation and basic care when Lumpy_Skin_Disease is detected

### Requirement 7: Crop Pest Detection

**User Story:** As a crop farmer, I want to identify pests damaging my crops by photographing affected leaves or plants, so that I can apply appropriate pest control measures before significant crop loss occurs.

#### Acceptance Criteria

1. WHEN an image of crop damage is captured, THE Edge_Module SHALL identify common pests including aphids, bollworms, and leaf miners
2. WHEN Pest_Detection is performed, THE Detection_Result SHALL specify the pest species and affected crop part
3. THE Edge_Module SHALL detect pests on major Indian crops including rice, wheat, cotton, and vegetables
4. WHEN pest damage is in early stages, THE Edge_Module SHALL detect with minimum 75% accuracy
5. WHEN multiple pest types are present, THE Detection_Result SHALL list all identified pests with individual confidence scores
6. THE System SHALL provide offline first-aid recommendations for detected pests

### Requirement 8: Low-Bandwidth Optimization

**User Story:** As a farmer in an area with only 2G connectivity, I want to receive detailed agricultural advice without consuming excessive data or waiting long periods, so that I can afford to use the service regularly.

#### Acceptance Criteria

1. WHEN Connectivity_State is 2G, THE System SHALL compress Multimodal_Input before transmission to use maximum 500KB per request
2. WHEN sending images to Agent_Module, THE System SHALL resize and optimize images while preserving diagnostic quality
3. WHEN receiving Advisory_Response, THE System SHALL use text format primarily and minimize media downloads
4. THE System SHALL implement progressive loading for Advisory_Response content based on available bandwidth
5. WHEN network speed is below 50 kbps, THE System SHALL warn users and suggest offline-only mode
6. THE System SHALL track data usage and display consumption statistics to users

### Requirement 9: User Interface for Low-Literacy Users

**User Story:** As a farmer with limited formal education, I want to use the app through simple visual icons and voice commands, so that I can access all features without needing to read complex instructions.

#### Acceptance Criteria

1. THE System SHALL provide a camera-first interface with large capture button as primary action
2. WHEN displaying Detection_Result, THE System SHALL use visual indicators (colors, icons) alongside text
3. THE System SHALL support voice navigation for all primary features
4. WHEN showing Advisory_Response, THE System SHALL use simple language and visual diagrams
5. THE System SHALL provide audio playback of all text content for accessibility
6. WHEN errors occur, THE System SHALL display simple visual messages with suggested actions

### Requirement 10: Model Management and Updates

**User Story:** As a system administrator, I want to update disease detection models remotely when new diseases emerge or model accuracy improves, so that farmers always have access to the latest diagnostic capabilities.

#### Acceptance Criteria

1. WHEN Connectivity_State is available, THE System SHALL check for TFLite_Model updates from the server
2. WHEN a new model version is available, THE System SHALL download and cache it in background without disrupting usage
3. WHEN model update is complete, THE System SHALL switch to the new TFLite_Model seamlessly
4. THE System SHALL maintain the previous model version until new model is verified functional
5. WHEN model download fails, THE System SHALL retry with exponential backoff up to 3 attempts
6. THE System SHALL allow users to manually trigger model updates when connected to WiFi

### Requirement 11: Data Privacy and Security

**User Story:** As a farmer concerned about privacy, I want my farm images and location data to be processed securely without being shared with unauthorized parties, so that I can trust the system with sensitive agricultural information.

#### Acceptance Criteria

1. WHEN processing images in Edge_Module, THE System SHALL not transmit any data outside the device
2. WHEN Connectivity_State is available and user opts to use Agent_Module, THE System SHALL encrypt Multimodal_Input before transmission
3. THE System SHALL request explicit user consent before sending any data to cloud services
4. WHEN storing Detection_Result locally, THE System SHALL encrypt sensitive farm data
5. THE System SHALL not collect or transmit user location data without explicit permission
6. WHEN user deletes data, THE System SHALL remove all local copies and request cloud deletion

### Requirement 12: Offline Advisory Cache

**User Story:** As a farmer who frequently encounters the same diseases, I want to access previously received advice offline, so that I can refer to treatment recommendations without needing internet connectivity each time.

#### Acceptance Criteria

1. WHEN Advisory_Response is received from Agent_Module, THE System SHALL cache it locally with associated Detection_Result
2. WHEN the same disease is detected offline, THE System SHALL retrieve and display cached Advisory_Response
3. THE System SHALL store up to 50 most recent Advisory_Response entries locally
4. WHEN cache storage exceeds limit, THE System SHALL remove oldest entries using LRU policy
5. WHEN displaying cached advice, THE System SHALL indicate the date it was originally received
6. THE System SHALL allow users to manually save important Advisory_Response as favorites for permanent offline access

### Requirement 13: Performance Monitoring and Feedback

**User Story:** As a product manager, I want to collect anonymous usage statistics and model accuracy feedback, so that we can continuously improve the system based on real-world performance.

#### Acceptance Criteria

1. WHEN Detection_Result is generated, THE System SHALL log confidence scores and processing time locally
2. WHEN Connectivity_State is available, THE System SHALL sync anonymous performance metrics to analytics service
3. THE System SHALL allow users to provide feedback on Detection_Result accuracy
4. WHEN user marks Detection_Result as incorrect, THE System SHALL collect corrected labels for model improvement
5. THE System SHALL track feature usage patterns to identify most valuable capabilities
6. WHEN collecting feedback, THE System SHALL not transmit personally identifiable information

### Requirement 14: Progressive Web App Architecture

**User Story:** As a farmer with limited storage on my phone, I want to use the app without installing a large native application, so that I can access the service without storage constraints.

#### Acceptance Criteria

1. THE System SHALL be implemented as a Progressive Web App (PWA) installable from browser
2. WHEN installed, THE System SHALL function like a native app with home screen icon and full-screen mode
3. THE System SHALL cache all essential assets for offline functionality using service workers
4. WHEN app updates are available, THE System SHALL download them in background and prompt user to refresh
5. THE System SHALL work across Chrome, Firefox, and Safari mobile browsers
6. WHEN device storage is low, THE System SHALL provide options to reduce cache size

### Requirement 15: Agent Reasoning and Context

**User Story:** As a farmer seeking advice, I want the AI agent to understand my specific situation including crop type, region, and season, so that recommendations are relevant to my actual farming context.

#### Acceptance Criteria

1. WHEN user first uses Agent_Module, THE System SHALL collect farm profile including location, crops grown, and livestock types
2. WHEN generating Advisory_Response, THE Agent_Module SHALL incorporate user's farm profile for contextual recommendations
3. THE Agent_Module SHALL consider seasonal factors and regional agricultural practices in advice
4. WHEN multiple interactions occur, THE Agent_Module SHALL maintain conversation context for follow-up questions
5. THE Agent_Module SHALL provide reasoning for recommendations to build farmer trust
6. WHEN suggesting treatments, THE Agent_Module SHALL consider locally available resources and affordability
