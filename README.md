{
  "name": "Vibe Music",
  "short_name": "Vibe",
  "start_url": ".",
  "display": "standalone",
  "background_color": "#667eea",
  "theme_color": "#667eea",
  "icons": [
    {
      "src": "https://cdn-icons-png.flaticon.com/512/727/727245.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}const songs = [
  { title: "Wimbo wa 1", artist: "Msanii A", src: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" },
  { title: "Wimbo wa 2", artist: "Msanii B", src: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3" },
  { title: "Wimbo wa 3", artist: "Msanii C", src: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3" }
];

let songIndex = 0;

const audio = document.getElementById('audio');
const playBtn = document.getElementById('playBtn');
const prevBtn = document.getElementById('prevBtn');
const nextBtn = document.getElementById('nextBtn');
const songTitle = document.getElementById('songTitle');
const songArtist = document.getElementById('songArtist');
const progressBar = document.getElementById('progressBar');
const playlist = document.getElementById('playlist');

function loadSong(song) {
  songTitle.textContent = song.title;
  songArtist.textContent = song.artist;
  audio.src = song.src;
}

function playSong() {
  audio.play();
  playBtn.textContent = '⏸️';
}

function pauseSong() {
  audio.pause();
  playBtn.textContent = '▶️';
}

function nextSong() {
  songIndex = (songIndex + 1) % songs.length;
  loadSong(songs[songIndex]);
  playSong();
  updatePlaylist();
}

function prevSong() {
  songIndex = (songIndex - 1 + songs.length) % songs.length;
  loadSong(songs[songIndex]);
  playSong();
  updatePlaylist();
}

function updatePlaylist() {
  playlist.innerHTML = '';
  songs.forEach((song, index) => {
    const div = document.createElement('div');
    div.className = 'song-item' + (index === songIndex? ' active' : '');
    div.innerHTML = `<strong>${song.title}</strong><br><small>${song.artist}</small>`;
    div.onclick = () => {
      songIndex = index;
      loadSong(song);
      playSong();
      updatePlaylist();
    };
    playlist.appendChild(div);
  });
}

playBtn.onclick = () => {
  audio.paused? playSong() : pauseSong();
};

nextBtn.onclick = nextSong;
prevBtn.onclick = prevSong;

audio.ontimeupdate = () => {
  progressBar.value = (audio.currentTime / audio.duration) * 100 || 0;
};

progressBar.oninput = () => {
  audio.currentTime = (progressBar.value / 100) * audio.duration;
};

audio.onended = nextSong;

loadSong(songs[0]);
updatePlaylist();<!DOCTYPE html>
<html lang="sw">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vibe Music</title>
  <link rel="stylesheet" href="style.css">
  <link rel="manifest" href="manifest.json">
</head>
<body>
  <div class="app">
    <header>
      <h1>🎵 Vibe</h1>
      <p>Sikiliza muziki wako kwa style</p>
    </header>

    <div class="player">
      <div class="song-info">
        <h2 id="songTitle">Chagua Wimbo</h2>
        <p id="songArtist">-</p>
      </div>

      <audio id="audio" src=""></audio>

      <div class="controls">
        <button id="prevBtn">⏮</button>
        <button id="playBtn">▶️</button>
        <button id="nextBtn">⏭</button>
      </div>

      <div class="progress">
        <input type="range" id="progressBar" value="0" max="100">
      </div>
    </div>

    <div class="playlist" id="playlist"></div>
  </div>

  <script src="script.js"></script>
</body>
</html>
