<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Photo Uploader</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body {
      background-color: #f8f9fa;
    }
    .container {
      max-width: 600px;
      margin: 50px auto;
      text-align: center;
    }
    .upload-btn {
      margin: 20px 0;
    }
    .preview {
      margin-top: 20px;
    }
    .preview img {
      max-width: 100%;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Photo Uploader</h1>
    <p>Upload your photos and store them in Google Drive.</p>
    <input type="file" id="fileInput" accept="image/*" multiple>
    <button class="btn btn-primary upload-btn" onclick="uploadFiles()">Upload Photos</button>
    <div class="preview" id="preview"></div>
  </div>
  <script>
    const webAppUrl = "https://script.google.com/macros/s/AKfycbwB7dIM7_cINmnpXRIz2QrI6uaRBTA_txXpMoeFec-Pdww2tv1MlJf5v_RxptVp0dMuQg/exec"; // Replace with your Google Apps Script URL

    function uploadFiles() {
      const fileInput = document.getElementById('fileInput');
      const files = fileInput.files;
      if (files.length === 0) {
        alert("Please select at least one file.");
        return;
      }

      const preview = document.getElementById('preview');
      preview.innerHTML = ""; // Clear previous previews

      Array.from(files).forEach(file => {
        const reader = new FileReader();
        reader.onload = (e) => {
          const img = document.createElement('img');
          img.src = e.target.result;
          preview.appendChild(img);
        };
        reader.readAsDataURL(file);

        // Upload file to Google Drive
        const formData = new FormData();
        formData.append('file', file);
        fetch(webAppUrl, {
          method: 'POST',
          body: formData
        })
        .then(response => response.text())
        .then(result => {
          console.log(result);
        })
        .catch(error => {
          console.error('Error uploading file:', error);
        });
      });
    }
  </script>
</body>
</html>
