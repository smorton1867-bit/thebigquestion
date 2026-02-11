<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Chloe, Will You Be My Valentine?</title>
<style>
  body {
    font-family: 'Arial', sans-serif;
    text-align: center;
    background: linear-gradient(to bottom right, #ffc1cc, #ffe4e1);
    margin: 0; padding: 0;
  }
  h1, h2, p {
    color: #ff3366;
  }
  #loveLetter, #trivia, #valentineQuestion, #finalMessage {
    margin-top: 50px;
    display: none;
  }
  button {
    margin: 10px;
    padding: 10px 20px;
    font-size: 1em;
    border-radius: 10px;
    border: none;
    cursor: pointer;
    background-color: #ff6699;
    color: white;
  }
  button:hover { background-color: #ff3366; }
  #noBtn { position: relative; }
  canvas { position: fixed; top:0; left:0; pointer-events:none; }
</style>
</head>
<body>

<audio id="bgMusic" src="milestone.mp3" autoplay loop></audio>

<div id="loveLetter">
  <h2>My Love Letter 💌</h2>
  <p>Chloe, I just want you to know how much I love you. From the moment we met at college, you've made my life brighter. You’re my favorite person in the world and I can't wait to ask you this…</p>
  <button onclick="startTrivia()">Open Trivia</button>
</div>

<div id="trivia">
  <h2>Trivia About Us!</h2>
  <div id="questionArea"></div>
</div>

<div id="valentineQuestion">
  <h2>Will you be my Valentine? 💖</h2>
  <button id="yesBtn" onclick="showFinalMessage()">Yes!</button>
  <button id="noBtn">No...</button>
</div>

<div id="finalMessage">
  <canvas id="confetti"></canvas>
  <h1>Yay! Chloe, you said yes! 💕</h1>
</div>

<script>
  // ===== Show love letter first =====
  document.getElementById('loveLetter').style.display = 'block';

  // ===== Trivia Section =====
  const questions = [
    {q:"Where did we meet?", a:["School","Collage","Cafe"], correct:1},
    {q:"Chloe's famous quote?", a:["Tallened","Hello World","Carpe Diem"], correct:0}
  ];
  let currentQ = 0;
  const questionArea = document.getElementById('questionArea');

  function startTrivia(){
    document.getElementById('loveLetter').style.display='none';
    document.getElementById('trivia').style.display='block';
    loadQuestion();
  }

  function loadQuestion(){
    const q = questions[currentQ];
    questionArea.innerHTML = `<p>${q.q}</p>`;
    q.a.forEach((ans,i)=>{
      const btn = document.createElement('button');
      btn.textContent=ans;
      btn.onclick = ()=>{
        if(i===q.correct){
          currentQ++;
          if(currentQ<questions.length) loadQuestion();
          else { document.getElementById('trivia').style.display='none'; document.getElementById('valentineQuestion').style.display='block'; }
        } else alert("Try again 💖");
      };
      questionArea.appendChild(btn);
    });
  }

  // ===== No button moves =====
  const noBtn = document.getElementById('noBtn');
  noBtn.addEventListener('mouseover', ()=>{
    const x = Math.random()*70 + 10;
    const y = Math.random()*70 + 10;
    noBtn.style.transform = `translate(${x}px, ${y}px)`;
  });

  // ===== Confetti =====
  const finalMessage = document.getElementById('finalMessage');
  const confettiCanvas = document.getElementById('confetti');
  const ctx = confettiCanvas.getContext('2d');
  confettiCanvas.width = window.innerWidth;
  confettiCanvas.height = window.innerHeight;

  let particles = [];
  function createConfetti(){
    for(let i=0;i<150;i++){
      particles.push({x:Math.random()*window.innerWidth,y:Math.random()*window.innerHeight,r:Math.random()*6+4,color:`hsl(${Math.random()*360},100%,50%)`,tilt:Math.random()*10-10,tiltAngle:0, tiltAngleIncrement:Math.random()*0.07+0.05});
    }
  }

  function drawConfetti(){
    ctx.clearRect(0,0,window.innerWidth,window.innerHeight);
    particles.forEach(p=>{
      ctx.beginPath();
      ctx.lineWidth = p.r/2;
      ctx.strokeStyle = p.color;
      ctx.moveTo(p.x+p.tilt+p.r/4,p.y);
      ctx.lineTo(p.x+p.tilt,p.y+p.tilt+p.r/4);
      ctx.stroke();
      p.tiltAngle += p.tiltAngleIncrement;
      p.y += (Math.cos(p.tiltAngle)+3+p.r/2)/2;
      p.x += Math.sin(p.tiltAngle);
      p.tilt = Math.sin(p.tiltAngle)*15;
      if(p.y>window.innerHeight){p.y=-10;p.x=Math.random()*window.innerWidth;}
    });
    requestAnimationFrame(drawConfetti);
  }

  function showFinalMessage(){
    document.getElementById('valentineQuestion').style.display='none';
    finalMessage.style.display='block';
    createConfetti();
    drawConfetti();
  }
</script>

</body>
</html>
