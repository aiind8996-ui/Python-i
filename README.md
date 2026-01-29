<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mini YouTube</title>

<style>
body{
    margin:0;
    font-family: Arial, sans-serif;
    background:#f9f9f9;
}
header{
    background:#ff0000;
    color:white;
    padding:10px;
    display:flex;
    align-items:center;
}
header h1{
    margin:0 15px 0 10px;
}
header input{
    flex:1;
    padding:8px;
    font-size:16px;
}
header button{
    padding:8px 15px;
    margin-left:10px;
    cursor:pointer;
}

.container{
    display:flex;
}
.sidebar{
    width:200px;
    background:white;
    padding:10px;
    height:calc(100vh - 50px);
    border-right:1px solid #ddd;
}
.sidebar div{
    padding:10px;
    cursor:pointer;
}
.sidebar div:hover{
    background:#eee;
}

.main{
    flex:1;
    padding:15px;
}
.video{
    width:100%;
    height:400px;
    background:black;
}
iframe{
    width:100%;
    height:100%;
}
.videos{
    display:grid;
    grid-template-columns: repeat(auto-fill,minmax(200px,1fr));
    gap:15px;
    margin-top:20px;
}
.card{
    background:white;
    cursor:pointer;
}
.card img{
    width:100%;
}
.card p{
    padding:8px;
    font-size:14px;
}
</style>
</head>

<body>

<header>
    <h1>MiniYouTube</h1>
    <input id="search" placeholder="Search">
    <button onclick="searchVideo()">Search</button>
</header>

<div class="container">
    <div class="sidebar">
        <div onclick="home()">🏠 Home</div>
        <div onclick="openYT()">▶️ YouTube</div>
        <div onclick="alert('Feature coming soon')">📺 Subscriptions</div>
        <div onclick="alert('Feature coming soon')">📂 Library</div>
    </div>

    <div class="main">
        <div class="video">
            <iframe id="player"
            src="https://www.youtube.com/embed/dQw4w9WgXcQ"
            frameborder="0"
            allowfullscreen></iframe>
        </div>

        <div class="videos">
            <div class="card" onclick="play('dQw4w9WgXcQ')">
                <img src="https://img.youtube.com/vi/dQw4w9WgXcQ/0.jpg">
                <p>Sample Video 1</p>
            </div>

            <div class="card" onclick="play('9bZkp7q19f0')">
                <img src="https://img.youtube.com/vi/9bZkp7q19f0/0.jpg">
                <p>Sample Video 2</p>
            </div>

            <div class="card" onclick="play('3tmd-ClpJxA')">
                <img src="https://img.youtube.com/vi/3tmd-ClpJxA/0.jpg">
                <p>Sample Video 3</p>
            </div>
        </div>
    </div>
</div>

<script>
function play(id){
    document.getElementById("player").src =
    "https://www.youtube.com/embed/" + id;
}

function searchVideo(){
    let q = document.getElementById("search").value;
    if(q.trim() !== ""){
        window.open(
          "https://www.youtube.com/results?search_query=" + q
        );
    }
}

function openYT(){
    window.open("https://www.youtube.com");
}

function home(){
    location.reload();
}
</script>

</body>
</html>
