<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PeluFiles - Transferencia sin límites</title>
    <style>
        :root {
            --bg-color: #121212;
            --surface-color: #1e1e1e;
            --primary-color: #bb86fc;
            --primary-variant: #3700b3;
            --text-color: #f1f1f1;
            --text-secondary: #b3b3b3;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            overflow-x: hidden;
        }
        
        .container {
            width: 90%;
            max-width: 800px;
            padding: 2rem;
            border-radius: 16px;
            background-color: var(--surface-color);
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
            opacity: 0;
            transform: translateY(20px);
            animation: fadeIn 0.8s ease-out forwards;
        }
        
        @keyframes fadeIn {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        h1 {
            font-size: 2.5rem;
            margin-bottom: 1rem;
            text-align: center;
            background: linear-gradient(90deg, var(--primary-color), #03dac6);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            animation: gradientShift 8s ease infinite;
        }
        
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        p {
            color: var(--text-secondary);
            text-align: center;
            margin-bottom: 2rem;
        }
        
        .upload-area {
            border: 2px dashed var(--primary-color);
            border-radius: 12px;
            padding: 3rem 2rem;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .upload-area::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--primary-color);
            opacity: 0;
            transition: opacity 0.3s ease;
            z-index: 0;
        }
        
        .upload-area:hover {
            border-color: #03dac6;
            transform: translateY(-5px);
        }
        
        .upload-area:hover::before {
            opacity: 0.05;
        }
        
        .upload-area.dragging {
            border-color: #03dac6;
            background-color: rgba(187, 134, 252, 0.1);
            transform: scale(1.02);
        }
        
        .upload-icon {
            font-size: 3rem;
            margin-bottom: 1rem;
            color: var(--primary-color);
            transition: transform 0.3s ease;
        }
        
        .upload-area:hover .upload-icon {
            transform: translateY(-10px);
        }
        
        .upload-text {
            position: relative;
            z-index: 1;
        }
        
        #file-input {
            display: none;
        }
        
        .progress-container {
            margin-top: 2rem;
            display: none;
        }
        
        .progress-bar {
            height: 6px;
            width: 100%;
            background-color: #333;
            border-radius: 3px;
            overflow: hidden;
            position: relative;
        }
        
        .progress {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, var(--primary-color), #03dac6);
            transition: width 0.3s ease;
            position: relative;
            border-radius: 3px;
        }
        
        .progress-label {
            margin-top: 0.5rem;
            text-align: center;
            color: var(--text-secondary);
        }
        
        .link-container {
            margin-top: 2rem;
            display: none;
            animation: fadeIn 0.5s ease-out forwards;
        }
        
        .file-link {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background-color: rgba(187, 134, 252, 0.1);
            padding: 1rem;
            border-radius: 8px;
            margin-bottom: 1rem;
        }
        
        .link-text {
            flex-grow: 1;
            padding: 0.5rem;
            background-color: rgba(0, 0, 0, 0.2);
            border-radius: 4px;
            border: none;
            color: var(--text-color);
            margin-right: 1rem;
            font-size: 0.9rem;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
        
        .copy-btn {
            padding: 0.5rem 1rem;
            background-color: var(--primary-color);
            color: #000;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
        }
        
        .copy-btn:hover {
            background-color: #03dac6;
            transform: translateY(-2px);
        }
        
        .copy-btn:active {
            transform: translateY(0);
        }
        
        .file-info {
            margin-bottom: 1rem;
            padding: 1rem;
            background-color: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .file-name {
            font-weight: bold;
            margin-right: 1rem;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
        
        .file-size {
            color: var(--text-secondary);
            font-size: 0.9rem;
        }
        
        .upload-new {
            display: block;
            margin: 0 auto;
            padding: 0.8rem 1.5rem;
            background-color: transparent;
            color: var(--primary-color);
            border: 2px solid var(--primary-color);
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
        }
        
        .upload-new:hover {
            background-color: rgba(187, 134, 252, 0.1);
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 1.5rem;
                width: 95%;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            .upload-area {
                padding: 2rem 1rem;
            }
            
            .file-link {
                flex-direction: column;
            }
            
            .link-text {
                margin-right: 0;
                margin-bottom: 1rem;
                width: 100%;
            }
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .loading-icon {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 0.8s linear infinite;
            margin-right: 8px;
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 1rem;
            background-color: var(--surface-color);
            color: var(--text-color);
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            z-index: 100;
            transform: translateX(120%);
            transition: transform 0.5s ease;
            display: flex;
            align-items: center;
        }
        
        .notification.show {
            transform: translateX(0);
        }
        
        .footer {
            margin-top: 3rem;
            color: var(--text-secondary);
            font-size: 0.9rem;
            text-align: center;
            opacity: 0.7;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>PeluFiles</h1>
        <p>Transfiere archivos sin límites. Súbelos y comparte el enlace.</p>
        
        <div class="upload-area" id="upload-area">
            <div class="upload-text">
                <div class="upload-icon">⬆️</div>
                <h3>Arrastra y suelta tu archivo aquí</h3>
                <p>O haz clic para seleccionar</p>
            </div>
            <input type="file" id="file-input">
        </div>
        
        <div class="progress-container" id="progress-container">
            <div class="progress-bar">
                <div class="progress" id="progress"></div>
            </div>
            <div class="progress-label" id="progress-label">Subiendo archivo... 0%</div>
        </div>
        
        <div class="link-container" id="link-container">
            <div class="file-info">
                <div class="file-name" id="file-name">archivo.pdf</div>
                <div class="file-size" id="file-size">2.5 MB</div>
            </div>
            <div class="file-link">
                <input type="text" class="link-text" id="file-link" readonly>
                <button class="copy-btn" id="copy-btn">Copiar enlace</button>
            </div>
            <button class="upload-new" id="upload-new">Subir otro archivo</button>
        </div>
    </div>
    
    <div class="footer">
        PeluFiles © 2025 - Transferencia de archivos sin límites
    </div>
    
    <div class="notification" id="notification">
        <div class="loading-icon"></div>
        <span id="notification-text">Enlace copiado al portapapeles</span>
    </div>
    
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const uploadArea = document.getElementById('upload-area');
            const fileInput = document.getElementById('file-input');
            const progressContainer = document.getElementById('progress-container');
            const progressBar = document.getElementById('progress');
            const progressLabel = document.getElementById('progress-label');
            const linkContainer = document.getElementById('link-container');
            const fileLink = document.getElementById('file-link');
            const copyBtn = document.getElementById('copy-btn');
            const uploadNew = document.getElementById('upload-new');
            const notification = document.getElementById('notification');
            const notificationText = document.getElementById('notification-text');
            const fileName = document.getElementById('file-name');
            const fileSize = document.getElementById('file-size');
            
            // Lógica para arrastrar y soltar
            uploadArea.addEventListener('dragover', function(e) {
                e.preventDefault();
                uploadArea.classList.add('dragging');
            });
            
            uploadArea.addEventListener('dragleave', function() {
                uploadArea.classList.remove('dragging');
            });
            
            uploadArea.addEventListener('drop', function(e) {
                e.preventDefault();
                uploadArea.classList.remove('dragging');
                if (e.dataTransfer.files.length) {
                    handleFile(e.dataTransfer.files[0]);
                }
            });
            
            uploadArea.addEventListener('click', function() {
                fileInput.click();
            });
            
            fileInput.addEventListener('change', function() {
                if (fileInput.files.length) {
                    handleFile(fileInput.files[0]);
                }
            });
            
            copyBtn.addEventListener('click', function() {
                fileLink.select();
                document.execCommand('copy');
                showNotification('Enlace copiado al portapapeles');
            });
            
            uploadNew.addEventListener('click', function() {
                linkContainer.style.display = 'none';
                uploadArea.style.display = 'block';
                progressContainer.style.display = 'none';
                progressBar.style.width = '0%';
            });
            
            function handleFile(file) {
                // Mostrar información del archivo
                fileName.textContent = file.name;
                fileSize.textContent = formatFileSize(file.size);
                
                // Mostrar progreso
                uploadArea.style.display = 'none';
                progressContainer.style.display = 'block';
                
                // Simular subida (en una implementación real, aquí se subiría al servidor)
                let progress = 0;
                const interval = setInterval(() => {
                    progress += Math.random() * 10;
                    if (progress >= 100) {
                        progress = 100;
                        clearInterval(interval);
                        completeUpload(file);
                    }
                    progressBar.style.width = progress + '%';
                    progressLabel.textContent = `Subiendo archivo... ${Math.round(progress)}%`;
                }, 300);
            }
            
            function completeUpload(file) {
                // Generar link único (simulado)
                const uniqueId = Math.random().toString(36).substring(2, 15);
                const downloadUrl = `https://pelufiles.github.io/download/${uniqueId}/${encodeURIComponent(file.name)}`;
                
                // Mostrar sección de enlace
                setTimeout(() => {
                    progressContainer.style.display = 'none';
                    linkContainer.style.display = 'block';
                    fileLink.value = downloadUrl;
                    showNotification('¡Archivo subido correctamente!');
                }, 500);
            }
            
            function formatFileSize(bytes) {
                if (bytes === 0) return '0 Bytes';
                const k = 1024;
                const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB'];
                const i = Math.floor(Math.log(bytes) / Math.log(k));
                return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
            }
            
            function showNotification(text) {
                notificationText.textContent = text;
                notification.classList.add('show');
                setTimeout(() => {
                    notification.classList.remove('show');
                }, 3000);
            }
            
            // Para la página de descarga (simulada en esta demo)
            const urlParams = new URLSearchParams(window.location.search);
            const downloadId = urlParams.get('id');
            if (downloadId) {
                // Mostrar página de descarga (implementar según sea necesario)
            }
        });
    </script>
</body>
</html>
