# First-repository-
Frontend development 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Player</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="player">

    <div class="cover">
        <img id="cover" src="https://via.placeholder.com/250" alt="cover">
    </div>

    <h2 id="title">Song Title</h2>
    <p id="artist">Artist Name</p>

    <audio id="audio"></audio>

    <div class="progress-container">
        <span id="current-time">0:00</span>

        <input
            type="range"
            id="progress"
            value="0"
            min="0"
            max="100"
        >

        <span id="duration">0:00</span>
    </div>

    <div class="controls">
        <button id="prev">⏮</button>
        <button id="play">▶</button>
        <button id="next">⏭</button>
    </div>

    <div class="volume">
        <label>🔊</label>
        <input
            type="range"
            id="volume"
            min="0"
            max="1"
            step="0.1"
            value="1"
        >
    </div>

    <div class="playlist">
        <h3>Playlist</h3>
        <ul id="playlist"></ul>
    </div>

</div>

<script src="script.js"></script>
</body>
</html>

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial, sans-serif;
}

body{
    background:#121212;
    color:white;
    display:flex;
    justify-content:center;
    align-items:center;
    min-height:100vh;
}

.player{
    width:350px;
    background:#1e1e1e;
    padding:20px;
    border-radius:20px;
    text-align:center;
    box-shadow:0 0 20px rgba(0,0,0,0.5);
}

.cover img{
    width:250px;
    height:250px;
    border-radius:15px;
    object-fit:cover;
}

h2{
    margin-top:15px;
}

p{
    color:#aaa;
    margin-bottom:15px;
}

.progress-container{
    display:flex;
    align-items:center;
    gap:10px;
    margin:20px 0;
}

#progress{
    flex:1;
}

.controls{
    display:flex;
    justify-content:center;
    gap:20px;
    margin:20px 0;
}

.controls button{
    width:60px;
    height:60px;
    border:none;
    border-radius:50%;
    font-size:24px;
    cursor:pointer;
    background:#ff4d4d;
    color:white;
    transition:0.3s;
}

.controls button:hover{
    transform:scale(1.1);
}

.volume{
    margin:20px 0;
}

.volume input{
    width:80%;
}

.playlist{
    margin-top:20px;
    text-align:left;
}

.playlist ul{
    list-style:none;
}

.playlist li{
    padding:10px;
    cursor:pointer;
    border-bottom:1px solid #333;
}

.playlist li:hover{
    background:#333;
}

const songs = [
{
    title: "Song One",
    artist: "Artist One",
    src: "songs/song1.mp3",
    cover: "https://picsum.photos/300?1"
},
{
    title: "Song Two",
    artist: "Artist Two",
    src: "songs/song2.mp3",
    cover: "https://picsum.photos/300?2"
},
{
    title: "Song Three",
    artist: "Artist Three",
    src: "songs/song3.mp3",
    cover: "https://picsum.photos/300?3"
}
];

const audio = document.getElementById("audio");
const playBtn = document.getElementById("play");
const prevBtn = document.getElementById("prev");
const nextBtn = document.getElementById("next");

const title = document.getElementById("title");
const artist = document.getElementById("artist");
const cover = document.getElementById("cover");

const progress = document.getElementById("progress");
const currentTime = document.getElementById("current-time");
const duration = document.getElementById("duration");

const volume = document.getElementById("volume");
const playlist = document.getElementById("playlist");

let songIndex = 0;
let isPlaying = false;

loadSong(songIndex);

function loadSong(index) {
    title.textContent = songs[index].title;
    artist.textContent = songs[index].artist;
    audio.src = songs[index].src;
    cover.src = songs[index].cover;
}

function playSong() {
    audio.play();
    playBtn.textContent = "⏸";
    isPlaying = true;
}

function pauseSong() {
    audio.pause();
    playBtn.textContent = "▶";
    isPlaying = false;
}

playBtn.addEventListener("click", () => {
    if (isPlaying) {
        pauseSong();
    } else {
        playSong();
    }
});

nextBtn.addEventListener("click", () => {
    songIndex++;

    if (songIndex >= songs.length) {
        songIndex = 0;
    }

    loadSong(songIndex);
    playSong();
});

prevBtn.addEventListener("click", () => {
    songIndex--;

    if (songIndex < 0) {
        songIndex = songs.length - 1;
    }

    loadSong(songIndex);
    playSong();
});

audio.addEventListener("timeupdate", () => {

    const current = audio.currentTime;
    const total = audio.duration;

    progress.value = (current / total) * 100 || 0;

    currentTime.textContent = formatTime(current);
    duration.textContent = formatTime(total);
});

progress.addEventListener("input", () => {
    audio.currentTime =
        (progress.value / 100) * audio.duration;
});

volume.addEventListener("input", () => {
    audio.volume = volume.value;
});

function formatTime(time) {

    if (isNaN(time)) return "0:00";

    const min = Math.floor(time / 60);
    const sec = Math.floor(time % 60);

    return `${min}:${sec < 10 ? "0" + sec : sec}`;
}

audio.addEventListener("ended", () => {
    songIndex++;

    if (songIndex >= songs.length) {
        songIndex = 0;
    }

    loadSong(songIndex);
    playSong();
});

songs.forEach((song, index) => {

    const li = document.createElement("li");

    li.textContent =
        `${song.title} - ${song.artist}`;

    li.addEventListener("click", () => {
        songIndex = index;
        loadSong(songIndex);
        playSong();
    });

    playlist.appendChild(li);
});
