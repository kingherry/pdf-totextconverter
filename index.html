<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image/PDF to TeX & Word Converter</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 20px; }
        input, select, button { margin: 10px; padding: 10px; }
        canvas { display: none; }
    </style>
</head>
<body>
    <h1>Convert PNG, JPG, PDF to TeX & Word</h1>
    <input type="file" id="fileInput" accept="image/png, image/jpeg, application/pdf">
    <select id="format">
        <option value="tex">TeX</option>
        <option value="word">Word</option>
    </select>
    <button onclick="convertFile()">Convert</button>
    <a id="downloadLink" style="display:none">Download</a>
    <canvas id="canvas"></canvas>
    <script>
        function convertFile() {
            const fileInput = document.getElementById('fileInput');
            const format = document.getElementById('format').value;
            const file = fileInput.files[0];
            if (!file) return alert('Please select a file');
            
            const reader = new FileReader();
            reader.onload = function(event) {
                const img = new Image();
                img.onload = function() {
                    const canvas = document.getElementById('canvas');
                    const ctx = canvas.getContext('2d');
                    canvas.width = img.width;
                    canvas.height = img.height;
                    ctx.drawImage(img, 0, 0);
                    const text = extractText(canvas);
                    downloadFile(text, format, file.name);
                };
                img.src = event.target.result;
            };
            
            if (file.type === 'application/pdf') {
                alert('PDF conversion requires additional processing in JavaScript. Currently unsupported.');
            } else {
                reader.readAsDataURL(file);
            }
        }

        function extractText(canvas) {
            return "Sample extracted text. (OCR requires a JS OCR library)";
        }
        
        function downloadFile(text, format, filename) {
            let content = text;
            let ext = format === 'tex' ? '.tex' : '.doc';
            if (format === 'tex') content = `\\documentclass{article}\n\\begin{document}\n${text}\n\\end{document}`;
            
            const blob = new Blob([content], { type: 'text/plain' });
            const link = document.getElementById('downloadLink');
            link.href = URL.createObjectURL(blob);
            link.download = filename.replace(/\.[^.]+$/, '') + ext;
            link.style.display = 'block';
            link.innerText = 'Download ' + format.toUpperCase() + ' File';
        }
    </script>
</body>
</html>
