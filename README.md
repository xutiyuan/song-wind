# song-wind
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>音乐播放器</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #e0f7da;
        }

       .player {
            text-align: center;
        }

       .record {
            width: 300px;
            height: 300px;
            border-radius: 50%;
            margin: 20px auto;
            position: relative;
            animation: spin 5s linear infinite;
            animation-play-state: paused;
            background: black;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
        }

       .record img {
            width: 180px;
            height: 180px;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }

       .record-title {
            font-size: 24px;
            color: #333;
            margin-top: 10px;
        }

        @keyframes spin {
            from {
                transform: rotate(0deg);
            }

            to {
                transform: rotate(360deg);
            }
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            border: none;
            background-color: #4caf50;
            color: white;
            border-radius: 5px;
        }

        button:hover {
            background-color: #45a049;
        }

       .progress-container {
            width: 300px;
            margin: 10px auto;
            display: flex;
            align-items: center;
        }

       .progress-bar {
            flex: 1;
            height: 10px;
            background-color: #ddd;
            margin: 0 10px;
            border-radius: 5px;
            overflow: hidden;
        }

       .progress {
            height: 100%;
            background-color: #4caf50;
            width: 0%;
            border-radius: 5px;
        }
    </style>
</head>

<body>
    <div class="player">
        <div class="record">
            <img src="封面图.png" alt="唱片图片">
        </div>
        <div class="record-title">Wind</div>
        <button id="play-pause">播放</button>
        <div class="progress-container">
            <span id="current-time">0:00</span>
            <div class="progress-bar">
                <div class="progress" id="progress"></div>
            </div>
            <span id="total-time">0:00</span>
        </div>
        <audio id="music" src="wind_improved.mp3"></audio>
    </div>
    <script>
        const playPauseButton = document.getElementById('play-pause');
        const record = document.querySelector('.record');
        const music = document.getElementById('music');
        const progress = document.getElementById('progress');
        const currentTimeSpan = document.getElementById('current-time');
        const totalTimeSpan = document.getElementById('total-time');

        playPauseButton.addEventListener('click', function () {
            if (music.paused) {
                music.play()
                   .then(() => {
                        record.style.animationPlayState = 'running';
                        playPauseButton.textContent = '暂停';
                    })
                   .catch((error) => {
                        console.error('播放音乐时出错:', error);
                    });
            } else {
                music.pause();
                record.style.animationPlayState = 'paused';
                playPauseButton.textContent = '播放';
            }
        });

        music.addEventListener('loadedmetadata', function () {
            const formatTime = (time) => {
                const minutes = Math.floor(time / 60);
                const seconds = Math.floor(time % 60);
                return `${minutes}:${seconds.toString().padStart(2, '0')}`;
            };
            totalTimeSpan.textContent = formatTime(music.duration);
        });

        music.addEventListener('timeupdate', function () {
            const percent = (music.currentTime / music.duration) * 100;
            progress.style.width = percent + '%';

            const formatTime = (time) => {
                const minutes = Math.floor(time / 60);
                const seconds = Math.floor(time % 60);
                return `${minutes}:${seconds.toString().padStart(2, '0')}`;
            };

            currentTimeSpan.textContent = formatTime(music.currentTime);
        });
    </script>
</body>

</html>    
