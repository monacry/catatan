# YouTube to MP3 Converter - Full Stack Application

## 📋 Step-by-Step Implementation Guide

### **STEP 1: Setup Backend (FastAPI + Python)**

#### 1.1 Install Dependencies

Buat folder project dan install library yang diperlukan:

```bash
# Buat folder project
mkdir youtube-mp3-converter
cd youtube-mp3-converter

# Buat virtual environment
python -m venv venv

# Aktifkan virtual environment
# Windows:
venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate

# Install dependencies
pip install fastapi uvicorn python-multipart yt-dlp pydantic
```

#### 1.2 Buat File Backend: `main.py`

```python
from fastapi import FastAPI, HTTPException
from fastapi.middleware.cors import CORSMiddleware
from fastapi.responses import FileResponse
from pydantic import BaseModel
import yt_dlp
import os
import re
from pathlib import Path

app = FastAPI(title="YouTube to MP3 Converter API")

# Enable CORS untuk frontend
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Folder untuk menyimpan file MP3
DOWNLOAD_FOLDER = "downloads"
Path(DOWNLOAD_FOLDER).mkdir(exist_ok=True)

class URLRequest(BaseModel):
    url: str

class ConvertRequest(BaseModel):
    url: str
    quality: str

def extract_video_id(url: str) -> str:
    """Extract video ID dari URL YouTube"""
    patterns = [
        r'(?:youtube\.com\/watch\?v=|youtu\.be\/|youtube\.com\/embed\/)([^&\n?#]+)',
        r'^([a-zA-Z0-9_-]{11})$'
    ]
    for pattern in patterns:
        match = re.search(pattern, url)
        if match:
            return match.group(1)
    return None

@app.get("/")
def read_root():
    return {"message": "YouTube to MP3 Converter API", "status": "running"}

@app.post("/api/video-info")
async def get_video_info(request: URLRequest):
    """Mendapatkan informasi video YouTube"""
    try:
        video_id = extract_video_id(request.url)
        if not video_id:
            raise HTTPException(status_code=400, detail="URL YouTube tidak valid")
        
        ydl_opts = {
            'quiet': True,
            'no_warnings': True,
            'extract_flat': False,
        }
        
        with yt_dlp.YoutubeDL(ydl_opts) as ydl:
            info = ydl.extract_info(request.url, download=False)
            
            return {
                "id": info.get('id'),
                "title": info.get('title'),
                "thumbnail": info.get('thumbnail'),
                "duration": info.get('duration'),
                "channel": info.get('uploader'),
                "view_count": info.get('view_count'),
            }
    
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"Error: {str(e)}")

@app.post("/api/convert")
async def convert_to_mp3(request: ConvertRequest):
    """Konversi video YouTube ke MP3"""
    try:
        video_id = extract_video_id(request.url)
        if not video_id:
            raise HTTPException(status_code=400, detail="URL YouTube tidak valid")
        
        # Mapping kualitas
        quality_map = {
            "128": "128",
            "192": "192",
            "320": "320"
        }
        
        audio_quality = quality_map.get(request.quality, "192")
        
        # Konfigurasi yt-dlp
        ydl_opts = {
            'format': 'bestaudio/best',
            'postprocessors': [{
                'key': 'FFmpegExtractAudio',
                'preferredcodec': 'mp3',
                'preferredquality': audio_quality,
            }],
            'outtmpl': f'{DOWNLOAD_FOLDER}/%(title)s.%(ext)s',
            'quiet': True,
            'no_warnings': True,
        }
        
        with yt_dlp.YoutubeDL(ydl_opts) as ydl:
            info = ydl.extract_info(request.url, download=True)
            filename = ydl.prepare_filename(info)
            mp3_filename = filename.rsplit('.', 1)[0] + '.mp3'
            
            if not os.path.exists(mp3_filename):
                raise HTTPException(status_code=500, detail="File konversi gagal dibuat")
            
            return {
                "success": True,
                "filename": os.path.basename(mp3_filename),
                "title": info.get('title'),
            }
    
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"Error: {str(e)}")

@app.get("/api/download/{filename}")
async def download_file(filename: str):
    """Download file MP3"""
    file_path = os.path.join(DOWNLOAD_FOLDER, filename)
    
    if not os.path.exists(file_path):
        raise HTTPException(status_code=404, detail="File tidak ditemukan")
    
    return FileResponse(
        path=file_path,
        media_type='audio/mpeg',
        filename=filename
    )

# Cleanup endpoint (opsional)
@app.delete("/api/cleanup/{filename}")
async def cleanup_file(filename: str):
    """Hapus file setelah download"""
    file_path = os.path.join(DOWNLOAD_FOLDER, filename)
    try:
        if os.path.exists(file_path):
            os.remove(file_path)
            return {"success": True, "message": "File berhasil dihapus"}
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
```

#### 1.3 Install FFmpeg (PENTING!)

yt-dlp memerlukan FFmpeg untuk konversi audio:

**Windows:**
1. Download FFmpeg dari: https://ffmpeg.org/download.html
2. Extract dan tambahkan ke PATH environment variable

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install ffmpeg
```

**MacOS:**
```bash
brew install ffmpeg
```

#### 1.4 Jalankan Backend

```bash
uvicorn main:app --reload --port 8000
```

Backend akan berjalan di: `http://localhost:8000`

---

### **STEP 2: Setup Frontend (React)**

#### 2.1 Buat File Frontend: `index.html`

Buat folder `frontend` dan buat file `index.html`:

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YouTube to MP3 Converter</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
    <div id="root"></div>
    
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    
    <script type="text/babel">
        const { useState } = React;
        
        // Icon Components
        const Music = () => (
            <svg className="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3" />
            </svg>
        );
        
        const Download = () => (
            <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
            </svg>
        );
        
        const Loader = () => (
            <svg className="animate-spin w-5 h-5" fill="none" viewBox="0 0 24 24">
                <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
                <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
        );
        
        const AlertCircle = () => (
            <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
        );
        
        const CheckCircle = () => (
            <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
        );

        function App() {
            const [url, setUrl] = useState('');
            const [loading, setLoading] = useState(false);
            const [videoInfo, setVideoInfo] = useState(null);
            const [selectedQuality, setSelectedQuality] = useState('192');
            const [error, setError] = useState('');
            const [downloading, setDownloading] = useState(false);
            const [success, setSuccess] = useState('');

            const API_BASE_URL = 'http://localhost:8000';

            const qualities = [
                { value: '128', label: '128 kbps', size: 'Standard' },
                { value: '192', label: '192 kbps', size: 'High Quality' },
                { value: '320', label: '320 kbps', size: 'Premium' }
            ];

            const formatDuration = (seconds) => {
                const mins = Math.floor(seconds / 60);
                const secs = seconds % 60;
                return `${mins}:${secs.toString().padStart(2, '0')}`;
            };

            const handleFetchInfo = async () => {
                if (!url.trim()) {
                    setError('Masukkan URL YouTube terlebih dahulu');
                    return;
                }

                setLoading(true);
                setError('');
                setSuccess('');
                setVideoInfo(null);

                try {
                    const response = await fetch(`${API_BASE_URL}/api/video-info`, {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify({ url: url })
                    });

                    if (!response.ok) {
                        const errorData = await response.json();
                        throw new Error(errorData.detail || 'Gagal mengambil informasi video');
                    }

                    const data = await response.json();
                    setVideoInfo(data);
                } catch (err) {
                    setError(err.message || 'Gagal mengambil informasi video');
                } finally {
                    setLoading(false);
                }
            };

            const handleDownload = async () => {
                setDownloading(true);
                setError('');
                setSuccess('');

                try {
                    const response = await fetch(`${API_BASE_URL}/api/convert`, {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify({ 
                            url: url,
                            quality: selectedQuality 
                        })
                    });

                    if (!response.ok) {
                        const errorData = await response.json();
                        throw new Error(errorData.detail || 'Gagal mengkonversi video');
                    }

                    const data = await response.json();
                    
                    // Download file
                    const downloadUrl = `${API_BASE_URL}/api/download/${data.filename}`;
                    const link = document.createElement('a');
                    link.href = downloadUrl;
                    link.download = data.filename;
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);
                    
                    setSuccess('Download berhasil! File sedang diunduh...');
                    
                } catch (err) {
                    setError(err.message || 'Gagal mendownload file');
                } finally {
                    setDownloading(false);
                }
            };

            return (
                <div className="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 py-8 px-4">
                    <div className="max-w-3xl mx-auto">
                        {/* Header */}
                        <div className="text-center mb-8">
                            <div className="inline-flex items-center justify-center w-16 h-16 bg-gradient-to-br from-blue-500 to-purple-600 rounded-2xl mb-4 shadow-lg">
                                <Music />
                            </div>
                            <h1 className="text-4xl font-bold text-gray-800 mb-2">
                                YouTube to MP3
                            </h1>
                            <p className="text-gray-600">
                                Konversi video YouTube menjadi file MP3 berkualitas tinggi
                            </p>
                        </div>

                        {/* Main Card */}
                        <div className="bg-white rounded-3xl shadow-xl p-8 mb-6">
                            {/* URL Input */}
                            <div className="mb-6">
                                <label className="block text-sm font-semibold text-gray-700 mb-3">
                                    URL Video YouTube
                                </label>
                                <div className="flex gap-3">
                                    <input
                                        type="text"
                                        value={url}
                                        onChange={(e) => setUrl(e.target.value)}
                                        onKeyPress={(e) => e.key === 'Enter' && handleFetchInfo()}
                                        placeholder="https://www.youtube.com/watch?v=..."
                                        className="flex-1 px-4 py-3 border-2 border-gray-200 rounded-xl focus:outline-none focus:border-blue-500 transition-colors"
                                    />
                                    <button
                                        onClick={handleFetchInfo}
                                        disabled={loading}
                                        className="px-6 py-3 bg-gradient-to-r from-blue-500 to-purple-600 text-white rounded-xl font-semibold hover:from-blue-600 hover:to-purple-700 disabled:opacity-50 disabled:cursor-not-allowed transition-all shadow-md hover:shadow-lg"
                                    >
                                        {loading ? <Loader /> : 'Cari'}
                                    </button>
                                </div>
                            </div>

                            {/* Error Message */}
                            {error && (
                                <div className="mb-6 p-4 bg-red-50 border-l-4 border-red-500 rounded-lg flex items-start gap-3">
                                    <AlertCircle />
                                    <p className="text-red-700 text-sm">{error}</p>
                                </div>
                            )}

                            {/* Success Message */}
                            {success && (
                                <div className="mb-6 p-4 bg-green-50 border-l-4 border-green-500 rounded-lg flex items-start gap-3">
                                    <CheckCircle />
                                    <p className="text-green-700 text-sm">{success}</p>
                                </div>
                            )}

                            {/* Video Preview */}
                            {videoInfo && (
                                <div className="space-y-6">
                                    <div className="p-6 bg-gradient-to-br from-gray-50 to-gray-100 rounded-2xl">
                                        <div className="flex gap-4">
                                            <img
                                                src={videoInfo.thumbnail}
                                                alt={videoInfo.title}
                                                className="w-40 h-24 object-cover rounded-lg shadow-md"
                                            />
                                            <div className="flex-1 min-w-0">
                                                <h3 className="font-semibold text-gray-800 mb-2 line-clamp-2">
                                                    {videoInfo.title}
                                                </h3>
                                                <div className="flex items-center gap-4 text-sm text-gray-600">
                                                    <span className="flex items-center gap-1">
                                                        <CheckCircle />
                                                        {videoInfo.channel}
                                                    </span>
                                                    <span>Durasi: {formatDuration(videoInfo.duration)}</span>
                                                </div>
                                            </div>
                                        </div>
                                    </div>

                                    {/* Quality Selection */}
                                    <div>
                                        <label className="block text-sm font-semibold text-gray-700 mb-3">
                                            Pilih Kualitas Audio
                                        </label>
                                        <div className="grid grid-cols-3 gap-3">
                                            {qualities.map((quality) => (
                                                <button
                                                    key={quality.value}
                                                    onClick={() => setSelectedQuality(quality.value)}
                                                    className={`p-4 rounded-xl border-2 transition-all ${
                                                        selectedQuality === quality.value
                                                            ? 'border-blue-500 bg-blue-50 shadow-md'
                                                            : 'border-gray-200 hover:border-gray-300 bg-white'
                                                    }`}
                                                >
                                                    <div className="text-center">
                                                        <div className={`font-bold ${
                                                            selectedQuality === quality.value
                                                                ? 'text-blue-600'
                                                                : 'text-gray-800'
                                                        }`}>
                                                            {quality.label}
                                                        </div>
                                                        <div className={`text-xs mt-1 ${
                                                            selectedQuality === quality.value
                                                                ? 'text-blue-600'
                                                                : 'text-gray-500'
                                                        }`}>
                                                            {quality.size}
                                                        </div>
                                                    </div>
                                                </button>
                                            ))}
                                        </div>
                                    </div>

                                    {/* Download Button */}
                                    <button
                                        onClick={handleDownload}
                                        disabled={downloading}
                                        className="w-full py-4 bg-gradient-to-r from-green-500 to-emerald-600 text-white rounded-xl font-bold text-lg hover:from-green-600 hover:to-emerald-700 disabled:opacity-50 disabled:cursor-not-allowed transition-all shadow-lg hover:shadow-xl flex items-center justify-center gap-2"
                                    >
                                        {downloading ? (
                                            <>
                                                <Loader />
                                                Mengonversi & Mengunduh...
                                            </>
                                        ) : (
                                            <>
                                                <Download />
                                                Download MP3
                                            </>
                                        )}
                                    </button>
                                </div>
                            )}
                        </div>

                        {/* Info Footer */}
                        <div className="text-center text-sm text-gray-500">
                            <p className="mb-1">
                                ✅ Full-stack application dengan FastAPI backend
                            </p>
                            <p>
                                Pastikan Anda memiliki hak untuk mengunduh konten yang dimaksud.
                            </p>
                        </div>
                    </div>
                </div>
            );
        }

        ReactDOM.render(<App />, document.getElementById('root'));
    </script>
</body>
</html>
```

---

### **STEP 3: Menjalankan Aplikasi**

#### 3.1 Jalankan Backend
Buka terminal pertama:
```bash
cd youtube-mp3-converter
source venv/bin/activate  # atau venv\Scripts\activate di Windows
uvicorn main:app --reload --port 8000
```

#### 3.2 Jalankan Frontend
Buka terminal kedua dan jalankan simple HTTP server:
```bash
cd frontend
# Python 3
python -m http.server 3000
# Atau gunakan Live Server di VS Code
```

Buka browser: `http://localhost:3000`

---

## 📁 Struktur Folder Final

```
youtube-mp3-converter/
├── venv/                  # Virtual environment
├── downloads/             # Folder untuk file MP3 (auto-created)
├── main.py               # Backend FastAPI
├── requirements.txt      # Dependencies
└── frontend/
    └── index.html        # Frontend React
```

---

## 📦 File requirements.txt

```
fastapi==0.104.1
uvicorn==0.24.0
python-multipart==0.0.6
yt-dlp==2023.11.16
pydantic==2.5.0
```

---

## 🚀 Cara Menggunakan

1. **Paste URL YouTube** di input field
2. **Klik "Cari"** untuk melihat preview video
3. **Pilih kualitas audio** (128/192/320 kbps)
4. **Klik "Download MP3"** untuk mengkonversi dan download
5. File akan otomatis terdownload ke folder Downloads browser Anda

---

## ⚠️ Catatan Penting

1. **FFmpeg harus terinstall** - wajib untuk konversi audio
2. **CORS sudah diaktifkan** di backend untuk local development
3. Untuk production, gunakan environment variables untuk konfigurasi
4. Tambahkan rate limiting untuk mencegah abuse
5. Implementasikan cleanup otomatis untuk file lama
6. Pertimbangkan aspek legal sebelum deploy ke production

---

## 🔧 Troubleshooting

**Error: FFmpeg not found**
- Install FFmpeg dan pastikan ada di PATH

**Error: CORS**
- Pastikan backend berjalan di port 8000
- Cek konfigurasi CORS di main.py

**Error: yt-dlp**
- Update yt-dlp: `pip install -U yt-dlp`

**Download tidak berfungsi**
- Cek folder `downloads/` ada dan writable
- Cek console browser untuk error

---

## 🎉 Selesai!

Aplikasi YouTube to MP3 converter full-stack Anda sudah siap digunakan!
