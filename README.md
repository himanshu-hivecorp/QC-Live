# QC Live - Professional 24/7 Streaming Application

A powerful, self-hosted streaming application that enables 24/7 live streaming to YouTube, Twitch, and other RTMP platforms.

## Features

- 🎥 **24/7 Live Streaming** - Stream videos continuously with automatic looping
- 📺 **Multi-Platform Support** - Stream to YouTube, Twitch, Facebook, or any RTMP server
- 🎬 **Video Management** - Upload, organize, and manage your video library
- 📊 **Real-time Statistics** - Monitor stream health, bitrate, FPS, and quality
- 🔄 **Multiple Concurrent Streams** - Run multiple streams simultaneously
- 🎯 **Stream Quality Options** - Choose between 720p (2 Mbps) or 1080p (3.5 Mbps)
- 🛡️ **Secure Authentication** - Protected admin access
- 📱 **Responsive Design** - Works on desktop and mobile devices
## System Requirements

- Node.js 18+ 
- FFmpeg (installed and accessible in PATH)
- 4GB+ RAM recommended
- Sufficient bandwidth for streaming (3-5 Mbps upload per stream)

**Screenshots**
![Screenshot 2025-07-31 022633](https://github.com/user-attachments/assets/7f12a8f5-7249-455e-8c6b-75a5ddbca08e)
![Screenshot 2025-07-31 022858](https://github.com/user-attachments/assets/00269445-c2c8-4c48-be89-ac308646552e)
![Screenshot 2025-07-31 023001](https://github.com/user-attachments/assets/b9200fe6-4889-4f06-a2a3-162a5df9b906)
![Screenshot 2025-07-31 023038](https://github.com/user-attachments/assets/1e897400-89f7-4fc4-81cd-9c03e972fb23)
![Screenshot 2025-07-31 022540](https://github.com/user-attachments/assets/28e06e96-f9db-4efe-b017-1c490bbbe7c2)
![Screenshot 2025-07-31 022747](https://github.com/user-attachments/assets/acd23b90-b910-4f83-ba3b-8730fc1f5fd2)




## Quick Start

### 1. Clone the Repository
```bash
git clone https://github.com/your-repo/qc-live.git
cd qc-live
 ```

### 2. Install Dependencies
```bash
npm install
```

### 3. Configure Environment
Copy the example environment file:
```bash
cp .env.example .env.local
```

Edit `.env.local` with your settings:
- Generate a secure `SESSION_SECRET`
- Set your `ADMIN_USERNAME` 
- Generate password hash using bcrypt

### 4. Initialize Database
```bash
node scripts/setup-db.js
```

### 5. Start the Application

**Development:**
```bash
npm run dev
```

**Production:**
```bash
npm run build
npm start
```

The application will be available at `http://localhost:3000`

## Docker Deployment

### Using Docker Compose

```yaml
version: '3.8'

services:
  app:
    image: node:18-alpine
    container_name: qc-live
    working_dir: /app
    volumes:
      - .:/app
      - /app/node_modules
      - ./uploads:/app/uploads
      - ./data:/app/data
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    command: >
      sh -c "
        apk add --no-cache ffmpeg python3 make g++ &&
        npm install &&
        npm rebuild sqlite3 &&
        npm run build &&
        npm start
      "
    restart: unless-stopped
```

Start the container:
```bash
docker-compose up -d
```

## Production Deployment

### Using PM2

1. Install PM2 globally:
```bash
npm install -g pm2
```

2. Build the application:
```bash
npm run build
```

3. Start with PM2:
```bash
pm2 start npm --name "qc-live" -- start
pm2 save
pm2 startup
```

### Nginx Reverse Proxy

Example Nginx configuration:
```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Security Best Practices

1. **Change Default Credentials** - Always update the default admin credentials
2. **Use HTTPS** - Deploy behind HTTPS in production
3. **Firewall Rules** - Only expose necessary ports
4. **Regular Updates** - Keep dependencies updated
5. **Backup** - Regular backup of database and uploaded videos

## Streaming Setup

### YouTube
1. Go to YouTube Studio → Go Live
2. Copy your Stream Key
3. Create a new stream in QC Live with YouTube platform
4. Paste your stream key

### Twitch
1. Go to Creator Dashboard → Settings → Stream
2. Copy your Primary Stream Key
3. Use Custom RTMP in QC Live
4. Enter: `rtmp://live.twitch.tv/live/YOUR_STREAM_KEY`

## Troubleshooting

### FFmpeg Not Found
- Ensure FFmpeg is installed: `ffmpeg -version`
- Windows: Download from [ffmpeg.org](https://ffmpeg.org)
- Linux: `sudo apt-get install ffmpeg`
- Mac: `brew install ffmpeg`

### Stream Stops Unexpectedly
- Check system resources (CPU, RAM)
- Verify network stability
- Review application logs
- Ensure video files are properly encoded

### Upload Issues
- Check file permissions on uploads directory
- Verify disk space availability
- Check maximum file size settings

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License.

## Credits

Designed and developed by **Himanshu-HIVEcorp**  
GitHub: [https://github.com/himanshu-hivecorp](https://github.com/himanshu-hivecorp)

---

© 2024 QC Live. All rights reserved.
