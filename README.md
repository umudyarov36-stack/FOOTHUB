<!DOCTYPE html>
<html lang="kk">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FOOTHUB PRO</title>

<style>
body{
    margin:0;
    font-family:Arial;
    background:#0b0f14;
    color:white;
}

/* NAV */
nav{
    display:flex;
    justify-content:space-between;
    padding:15px;
    background:#111827;
}

nav h1{
    color:#00ff88;
}

/* LOGIN */
#login{
    width:300px;
    margin:100px auto;
    background:#1f2937;
    padding:20px;
    border-radius:10px;
    text-align:center;
}

input{
    width:90%;
    padding:10px;
    margin:10px 0;
    border:none;
    border-radius:5px;
}

button{
    padding:10px 15px;
    background:#00ff88;
    border:none;
    cursor:pointer;
    border-radius:6px;
    font-weight:bold;
}

/* MAIN */
.container{
    padding:20px;
}

.card{
    background:#1f2937;
    padding:15px;
    margin:10px 0;
    border-radius:10px;
}

.hidden{display:none;}

.search{
    margin:10px 0;
    width:100%;
    padding:10px;
}

/* DARK MODE */
.light{
    background:white;
    color:black;
}

.light .card{
    background:#eee;
    color:black;
}

.logout{
    background:red;
    color:white;
}
</style>
</head>

<body>

<!-- LOGIN -->
<div id="login">
    <h2>FOOTHUB LOGIN 🔐</h2>
    <input id="pass" type="password" placeholder="Admin password">
    <button onclick="login()">Login</button>
    <p id="err" style="color:red"></p>
</div>

<!-- MAIN -->
<div id="main" class="hidden">

<nav>
    <h1>⚽ FOOTHUB PRO</h1>
    <div>
        <button onclick="toggleMode()">🌙</button>
        <button onclick="logout()" class="logout">Logout</button>
    </div>
</nav>

<div class="container">

    <h2>🔎 Search</h2>
    <input class="search" id="search" onkeyup="filter()" placeholder="Team search">

    <h2>➕ Add Match (Admin)</h2>
    <input id="team" placeholder="Team name">
    <input id="score" placeholder="Score">
    <button onclick="addMatch()">Add</button>

    <div id="matches"></div>

</div>
</div>

<script>

const PASSWORD = "1234";

/* LOGIN */
function login(){
    let p = document.getElementById("pass").value;

    if(p === PASSWORD){
        localStorage.setItem("auth","ok");
        show();
    }else{
        document.getElementById("err").innerText="Wrong password!";
    }
}

function logout(){
    localStorage.removeItem("auth");
    location.reload();
}

function show(){
    document.getElementById("login").style.display="none";
    document.getElementById("main").classList.remove("hidden");
}

if(localStorage.getItem("auth")==="ok"){
    show();
}

/* DARK MODE */
function toggleMode(){
    document.body.classList.toggle("light");
}

/* MATCHS */
function addMatch(){
    let team = document.getElementById("team").value;
    let score = document.getElementById("score").value;

    if(team=="" || score=="") return;

    let data = JSON.parse(localStorage.getItem("matches") || "[]");

    data.push({team, score});

    localStorage.setItem("matches", JSON.stringify(data));

    render();

    document.getElementById("team").value="";
    document.getElementById("score").value="";
}

function render(){
    let data = JSON.parse(localStorage.getItem("matches") || "[]");

    let box = document.getElementById("matches");
    box.innerHTML="";

    data.forEach((m,i)=>{
        let div = document.createElement("div");
        div.className="card";
        div.innerHTML=`
            <h3>${m.team}</h3>
            <p>${m.score}</p>
            <button onclick="del(${i})">Delete</button>
        `;
        box.appendChild(div);
    });
}

function del(i){
    let data = JSON.parse(localStorage.getItem("matches") || "[]");
    data.splice(i,1);
    localStorage.setItem("matches", JSON.stringify(data));
    render();
}

/* SEARCH */
function filter(){
    let value = document.getElementById("search").value.toLowerCase();
    let cards = document.querySelectorAll(".card");

    cards.forEach(c=>{
        c.style.display = c.innerText.toLowerCase().includes(value) ? "block":"none";
    });
}

render();

</script>

</body>
</html># FOOTHUB
Футболные новости,тренды, резултаты мачти.
