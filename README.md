import os
import pytesseract
from flask import Flask, request, send_file
from werkzeug.utils import secure_filename
from pdf2image import convert_from_path
from PIL import Image

app = Flask(__name__)
UPLOAD_FOLDER = 'uploads'
RESULT_FOLDER = 'results'
os.makedirs(UPLOAD_FOLDER, exist_ok=True)
os.makedirs(RESULT_FOLDER, exist_ok=True)

def extract_text(image_path):
    return pytesseract.image_to_string(Image.open(image_path))

def convert_to_tex(text):
    return f"\\documentclass{{article}}\n\\begin{{document}}\n{text}\n\\end{{document}}"

def convert_to_word(text, output_path):
    from docx import Document
    doc = Document()
    doc.add_paragraph(text)
    doc.save(output_path)

@app.route('/', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        file = request.files['file']
        if file:
            filename = secure_filename(file.filename)
            file_path = os.path.join(UPLOAD_FOLDER, filename)
            file.save(file_path)
            
            if filename.lower().endswith('.pdf'):
                images = convert_from_path(file_path)
                text = "\n".join([pytesseract.image_to_string(img) for img in images])
            else:
                text = extract_text(file_path)
            
            if not text.strip():
                return "OCR failed. Please try again."
            
            format_type = request.form.get('format')
            if format_type == 'tex':
                output_path = os.path.join(RESULT_FOLDER, filename.rsplit('.', 1)[0] + '.tex')
                with open(output_path, 'w') as tex_file:
                    tex_file.write(convert_to_tex(text))
            else:
                output_path = os.path.join(RESULT_FOLDER, filename.rsplit('.', 1)[0] + '.docx')
                convert_to_word(text, output_path)
            
            return send_file(output_path, as_attachment=True)
    
    return '''
    <!doctype html>
    <html lang="en">
    <head><title>Image/PDF to TeX & Word Converter</title></head>
    <body>
        <h1>Upload PNG, JPG, or PDF</h1>
        <form method="post" enctype="multipart/form-data">
            <input type="file" name="file" required>
            <select name="format">
                <option value="tex">TeX</option>
                <option value="word">Word</option>
            </select>
            <input type="submit" value="Convert">
        </form>
    </body>
    </html>
    '''

if __name__ == '__main__':
    app.run(debug=True)
