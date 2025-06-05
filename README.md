# PixTrove

PixTrove is a web application designed to automate the organization of event photos by recognizing participants' faces. It features a Next.js frontend for user interaction and a Flask backend for image processing and face recognition.

## Key Features  
- **Face Recognition**: Identifies faces in event photos using a reference image.  
- **Automated Organization**: Creates folders named after participants and sorts their photos into respective folders.  
- **Manual Review**: Unmatched photos remain in the upload area, implicitly requiring manual review and categorization.
- **Efficient Workflow**: Saves time by eliminating the need for manual photo sorting.  

## How It Works  
1. **Prepare Reference Photos**:
   - Reference photos of participants are manually added to the `./reference_photos` directory. Each photo should ideally clearly show the face of an individual and be named in a way that identifies them (e.g., `JohnDoe.jpg`). This name will be used for the output folder.

2. **Upload Event Photos**:
   - Users navigate to the 'Organize' or 'Add Event' section of the web application.
   - Event photos are uploaded through the drag-and-drop interface. These photos are temporarily stored in the `./uploads` directory on the server.

3. **Automated Photo Processing & Organization**:
   - The Flask backend processes each uploaded photo from the `./uploads` directory.
   - It uses the `face_recognition` library to detect faces in the event photos and compare them against the faces in the `reference_photos` directory.
   - If a match is found, the event photo is moved to a subfolder within `./organised_photos` named after the corresponding reference photo's filename (e.g., `./organised_photos/JohnDoe/`).

4. **Access Organized Photos**:
   - Organized photos can be accessed directly from the `./organised_photos` directory on the server.

## Tech Stack

### Backend
- **Programming Language**: Python
- **Framework**: Flask
- **Face Recognition**: `face_recognition` (utilizes Dlib)
- **Image Processing**: OpenCV (cv2)

### Frontend
- **Framework**: Next.js (React)
- **Styling**: Tailwind CSS

### Storage
- **Photo Storage**: Local file system (for reference photos, uploaded event photos, and organized photos)

## Installation and Setup

Follow these steps to set up and run PixTrove locally:

### Prerequisites
- Python (3.8 or newer recommended)
- Node.js and npm (LTS version recommended)
- Git

### 1. Clone the Repository
```bash
git clone <repository_url>
cd <repository_directory_name>
```
(Replace `<repository_url>` with the actual URL and `<repository_directory_name>` with the folder name, e.g., `Event-Photo-Organizer-with-Face-Recognition`)

### 2. Backend Setup (Flask)
Navigate to the project root directory if you aren't already there.

a. **Create and Activate a Virtual Environment** (recommended):
   ```bash
   python -m venv env
   # On Windows
   .\env\Scripts\activate
   # On macOS/Linux
   source env/bin/activate
   ```

b. **Install Python Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
   *Note: The `requirements.txt` file is encoded in UTF-16 LE. If you encounter issues, you might need to convert it to UTF-8 first. One way to do this is to open it in a text editor like VS Code or Notepad++ and save it with UTF-8 encoding.*

### 3. Frontend Setup (Next.js)
Navigate to the project root directory (if not already there, though typically the Next.js app is at the root alongside `app.py`).

a. **Install Node.js Dependencies**:
   ```bash
   npm install
   ```

### 4. Prepare Directories
Ensure the following directories exist in the project root, or create them if they don't:
- `./reference_photos`: Place your reference images here.
- `./uploads`: This will be used for temporary storage of uploaded event photos.
- `./organised_photos`: Matched photos will be saved here.
*The Flask application (`app.py`) attempts to create these directories automatically if they are missing.*

### 5. Running the Application

a. **Run the Backend (Flask)**:
   Open a terminal, activate your Python virtual environment, and run:
   ```bash
   python app.py
   ```
   The Flask development server will typically start on `http://127.0.0.1:5000/`.

b. **Run the Frontend (Next.js)**:
   Open another terminal and run:
   ```bash
   npm run dev
   ```
   The Next.js development server will typically start on `http://localhost:3000/`.

c. **Access PixTrove**:
   Open your web browser and navigate to `http://localhost:3000`.

## API Endpoints (Flask Backend)

The Flask backend (`app.py`) provides the following API endpoints:

### `POST /upload`
- **Purpose**: Handles the upload of event photos for processing.
- **Method**: `POST`
- **Request**: Expects a multipart/form-data request with a file part named `file`.
  - The file should be an image with an allowed extension (`png`, `jpg`, `jpeg`).
- **Response**:
  - **Success (match found)**: JSON object with `message` (e.g., "File moved to <organized_directory>"), `matched_with` (filename of the reference photo), and `preview_url`.
    ```json
    {
      "message": "File moved to organised_photos/JohnDoe",
      "matched_with": "JohnDoe.jpg",
      "preview_url": "/preview/JohnDoe.jpg"
    }
    ```
  - **Success (no match found)**: JSON object with `message`: `"No matching faces found."`
  - **Success (no face in uploaded image)**: JSON object with `message`: `"No faces found in the uploaded image."`
  - **Error**: JSON object with `error` message (e.g., "No file part", "No selected file", "Invalid file type", or other processing errors) and a corresponding HTTP status code (e.g., 400).
    ```json
    {
      "error": "No file part"
    }
    ```

### `GET /preview/<filename>`
- **Purpose**: Serves a reference image file. This is used by the `/upload` endpoint's response to provide a direct link to the matched reference image, but can also be accessed directly.
- **Method**: `GET`
- **URL Parameter**: `filename` - The name of the reference image file located in the `reference_photos` directory.
- **Response**:
  - **Success**: The image file.
  - **Error (image not found)**: JSON object `{"error": "Image not found"}` with HTTP status code 404.

## Usage

Once the backend and frontend servers are running (see "Installation and Setup"), you can use PixTrove as follows:

1.  **Prepare Reference Photos**:
    *   Create a directory named `reference_photos` in the root of the project (if it doesn't exist).
    *   Add clear headshot photos of individuals you want the system to recognize into this `reference_photos` directory.
    *   Name the files descriptively, as these names will be used for the organized folders (e.g., `John_Doe.jpg`, `Jane_Smith.png`).

2.  **Access the Web Application**:
    *   Open your web browser and go to the frontend URL (typically `http://localhost:3000`).

3.  **Upload Event Photos**:
    *   Navigate to the "Organize" or "Add Event" page using the navigation links (e.g., "Let's Organise", "Get Started", or "Add Event" in the navbar).
    *   On this page, you will find a drag-and-drop area. Drag your event photos into this area, or click it to open a file dialog and select your photos.
    *   Click the "Upload" button to send the photos to the backend for processing.
    *   *(Note: As of the last review, the client-side JavaScript to trigger the upload from the 'Upload' button in `app/components/Previews.js` to the backend `/upload` endpoint was not fully implemented. This documentation describes the intended workflow.)*

4.  **Processing**:
    *   The Flask backend will process the uploaded photos. It will attempt to detect faces in them and compare these faces with the ones in your `reference_photos` directory.
    *   Uploaded photos are temporarily stored in the `uploads` directory during processing.

5.  **Find Organized Photos**:
    *   If a match is found, the corresponding event photo will be moved from the `uploads` directory to a subdirectory within `organised_photos`.
    *   The subdirectory will be named after the matched reference photo (e.g., if an event photo matches `reference_photos/John_Doe.jpg`, it will be moved to `organised_photos/John_Doe/`).
    *   You can access these organized photos directly from the `organised_photos` folder in your project directory.
    *   If no match is found, or no faces are detected in an uploaded photo, it might remain in the `uploads` directory or be handled as per the messages returned by the API (e.g., "No matching faces found," "No faces found in the uploaded image").

6.  **Manual Review (Implied)**:
    *   The current `README.md` mentions "Flags unmatched faces for manual categorization." While the backend logic in `app.py` doesn't explicitly create a 'manual review' folder, photos that are not matched or cause errors would implicitly require manual review by checking the `uploads` folder or API responses.

## Contributing
Contributions are welcome! Please fork this repository, make your changes, and submit a pull request.


