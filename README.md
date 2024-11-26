IPTV MALAY
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IPTV Player</title>
    <style>
        /* Gaya CSS */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            color: #333;
        }

        .container {
            max-width: 800px;
            margin: auto;
            padding: 20px;
        }

        #player {
            margin-bottom: 20px;
        }

        #video {
            width: 100%;
            height: auto;
            background: #000;
        }

        #playlist {
            background: #fff;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        #channel-list {
            list-style: none;
            padding: 0;
        }

        #channel-list li {
            padding: 10px;
            border-bottom: 1px solid #ddd;
            cursor: pointer;
        }

        #channel-list li:hover {
            background: #007bff;
            color: #fff;
        }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
</head>
<body>
    <div class="container">
        <h1>IPTV Player</h1>
        <div id="player">
            <video id="video" controls autoplay></video>
        </div>
        <div id="playlist">
            <h2>Available Channels</h2>
            <ul id="channel-list"></ul>
        </div>
    </div>

    <script>
        // Playlist URL
        const playlistUrl = "https://starlineiptv.blogspot.com/2024/11/iptv-malay-1bulan.html";

        // DOM Elements
        const video = document.getElementById("video");
        const channelList = document.getElementById("channel-list");

        // Load playlist and parse channels
        fetch(playlistUrl)
            .then(response => response.text())
            .then(data => {
                const channels = parseM3U(data);
                displayChannels(channels);
            })
            .catch(error => console.error("Error loading playlist:", error));

        // Parse M3U file
        function parseM3U(data) {
            const lines = data.split("\n");
            const channels = [];
            let currentChannel = null;

            lines.forEach(line => {
                if (line.startsWith("#EXTINF")) {
                    currentChannel = {
                        name: line.split(",")[1],
                        url: null
                    };
                } else if (line.startsWith("http") && currentChannel) {
                    currentChannel.url = line;
                    channels.push(currentChannel);
                    currentChannel = null;
                }
            });

            return channels;
        }

        // Display channel list
        function displayChannels(channels) {
            channels.forEach(channel => {
                const li = document.createElement("li");
                li.textContent = channel.name;
                li.addEventListener("click", () => playChannel(channel.url));
                channelList.appendChild(li);
            });
        }

        // Play selected channel
        function playChannel(url) {
            if (Hls.isSupported()) {
                const hls = new Hls();
                hls.loadSource(url);
                hls.attachMedia(video);
            } else if (video.canPlayType("application/vnd.apple.mpegurl")) {
                video.src = url;
            }
        }
    </script>
</body>
</html>


