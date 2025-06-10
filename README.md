
<!doctype html>
<html lang="en">
 <head>
  <meta charset="UTF-8">
  <title>Spin &amp; Win</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background: #f9f9f9;
      padding-top: 50px;
    }

    .wheel-container {
      position: relative;
      margin: auto;
      width: 300px;
      height: 300px;
    }

    .wheel {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      border: 10px solid #333;
      background: conic-gradient(
        #FFD700 0% 25%,
        #FF4500 25% 50%,
        #32CD32 50% 75%,
        #1E90FF 75% 100%
      );
      transition: transform 5s ease-out;
    }

    .pointer {
      position: absolute;
      top: -20px;
      left: 50%;
      transform: translateX(-50%);
      width: 0;
      height: 0;
      border-left: 15px solid transparent;
      border-right: 15px solid transparent;
      border-bottom: 30px solid red;
    }

    button {
      margin-top: 15px;
      padding: 10px 20px;
      font-size: 18px;
      cursor: pointer;
      border: none;
      border-radius: 5px;
      background-color: #007bff;
      color: white;
    }

    #result, #share-section, #final-message {
      margin-top: 30px;
      font-size: 20px;
    }

    .whatsapp-btn {
      background-color: #25D366;
    }

    .facebook-btn {
      background-color: #4267B2;
    }

    .reward-image {
      margin-top: 20px;
      max-width: 200px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.2);
    }
  </style>
 </head>
 <body>
  <h2>🎯 Spin &amp; Win Game</h2>
  <div class="wheel-container">
   <div class="pointer"></div>
   <div class="wheel" id="wheel"></div>
  </div><button onclick="spin()">🎯 Spin Now</button>
  <div id="result"></div><img id="reward-img" class="reward-image" src="" alt="" style="display:none;">
  <div id="share-section" style="display:none;">
   <p>🎁 Share this game with 10 friends to claim your reward!</p><button class="whatsapp-btn" onclick="shareGame('whatsapp')">📤 Share on WhatsApp</button> <button class="facebook-btn" onclick="shareGame('facebook')">📤 Share on Facebook</button>
   <p id="share-count">Shares: 0 / 10</p>
  </div>
  <div id="final-message" style="display:none;"></div><!-- Sounds -->
  <audio id="spin-sound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_c4decd4f1b.mp3" preload="auto"></audio>
  <audio id="win-sound" src="https://cdn.pixabay.com/download/audio/2021/09/23/audio_86252fc5fb.mp3" preload="auto"></audio>
  <script>
    let spinning = false;
    let prizeText = '';
    let shareCount = 0;

    const prizeImages = {
      "iPhone 15": "https://cdn.pixabay.com/photo/2022/11/07/23/35/iphone-7577895_1280.jpg",
      "$100 Gift Card": "https://cdn.pixabay.com/photo/2016/12/06/09/31/voucher-1887886_1280.jpg",
      "Free Spin": "https://cdn.pixabay.com/photo/2017/09/08/12/29/wheel-of-fortune-2729517_1280.jpg"
    };

    function spin() {
      if (spinning) return;
      spinning = true;

      const wheel = document.getElementById("wheel");
      const degrees = 3600 + Math.floor(Math.random() * 360);
      wheel.style.transform = `rotate(${degrees}deg)`;

      document.getElementById("result").innerText = "Spinning...";
      document.getElementById("reward-img").style.display = "none";
      document.getElementById("share-section").style.display = "none";
      document.getElementById("final-message").style.display = "none";

      document.getElementById("spin-sound").play();

      setTimeout(() => {
        const segments = ["iPhone 15", "$100 Gift Card", "Free Spin"];
        const selected = segments[Math.floor(Math.random() * segments.length)];
        prizeText = selected;

        document.getElementById("result").innerText = `🎉 You won: ${selected}`;
        document.getElementById("reward-img").src = prizeImages[selected];
        document.getElementById("reward-img").style.display = "block";
        document.getElementById("share-section").style.display = "block";
        shareCount = 0;
        updateShareCount();
        document.getElementById("win-sound").play();
        spinning = false;
      }, 5000);
    }

    function shareGame(platform) {
      const message = `🎯 I just won a ${prizeText} on this Spin & Win game! Try your luck: https://example.com`;

      if (platform === 'whatsapp') {
        window.open("https://wa.me/?text=" + encodeURIComponent(message), "_blank");
      } else if (platform === 'facebook') {
        window.open("https://www.facebook.com/sharer/sharer.php?u=https://example.com", "_blank");
      }

      shareCount++;
      updateShareCount();

      if (shareCount >= 10) {
        document.getElementById("final-message").innerText = `🎊 Congratulations! You can now claim your ${prizeText}!`;
        document.getElementById("final-message").style.display = "block";
        document.getElementById("share-section").style.display = "none";
      }
    }

    function updateShareCount() {
      document.getElementById("share-count").innerText = `Shares: ${shareCount} / 10`;
    }
  </script>
 </body>
</html>
