# AI-Visibility-Tracker
`AI Search Visibility Tracker built with Appwrite TablesDB &amp; VectorsDB for Appwrite Hackathon`
<!DOCTYPE html>
<html lang="en"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>AIVT demo</title>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@600;800&family=Instrument+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{--paper:#F6F7F2;--ink:#14213D;--sea:#0F766E;--flag:#F3D27A;--miss:#B4432F;--card:#fff;--line:rgba(20,33,61,.18);box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--paper:#0F172A;--ink:#E8ECF5;--sea:#2DD4BF;--flag:#7A5C00;--miss:#F08A74;--card:#17223B;--line:rgba(232,236,245,.2)}}
*{box-sizing:border-box}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;background:var(--paper);color:var(--ink);font:16px/1.55 "Instrument Sans",system-ui,sans-serif}
header{display:flex;justify-content:space-between;align-items:center;max-width:880px;margin:0 auto;padding:18px 20px}
.logo,h1,h2{font-family:Fraunces,Georgia,serif;font-weight:800}
.logo{font-size:22px;cursor:pointer}
nav button{background:none;border:0;color:var(--ink);font:inherit;text-decoration:underline;text-underline-offset:4px;cursor:pointer;margin-left:16px}
main{max-width:880px;margin:0 auto;padding:24px 20px 80px}
h1{font-size:clamp(32px,7vw,64px);line-height:1.05;margin:24px 0 16px;max-width:15em}
h2{font-size:26px;margin:40px 0 12px}
p.lead{max-width:34em;font-size:18px;opacity:.85}
.btn{background:var(--ink);color:var(--paper);border:0;border-radius:8px;padding:12px 24px;font:600 16px inherit;font-family:inherit;cursor:pointer;margin-top:20px}
.btn:hover{background:var(--sea);color:#fff}.btn:disabled{opacity:.6}
.btn:focus-visible,input:focus-visible,summary:focus-visible,nav button:focus-visible{outline:3px solid var(--sea);outline-offset:2px}
label{display:block;font-weight:500;font-size:14px;margin-top:18px}
input{width:100%;margin-top:6px;padding:10px 12px;border:1px solid var(--line);border-radius:8px;background:var(--card);color:var(--ink);font:inherit}
form{max-width:480px}
.top{display:flex;flex-wrap:wrap;gap:36px;align-items:center;margin-top:20px}
table{border-collapse:collapse;min-width:260px}
th,td{text-align:left;padding:8px 28px 8px 0;border-bottom:1px solid var(--line)}
details{background:var(--card);border:1px solid var(--line);border-radius:8px;padding:14px 16px;margin-bottom:12px}
summary{cursor:pointer;font-weight:500}
details p{margin:12px 0 0;font-size:14px;white-space:pre-wrap}
mark{background:var(--flag);color:inherit;border-radius:3px;padding:0 2px}
.ok{color:var(--sea)}.no{color:var(--miss)}
ul{list-style:none;padding:0;margin:0}li{display:flex;justify-content:space-between;padding:10px 0;border-bottom:1px solid var(--line)}
.note{font-size:13px;opacity:.7;margin-top:28px}
.scroll{overflow-x:auto}
@media (prefers-reduced-motion:no-preference){.ring{transition:stroke-dashoffset 1.1s ease}}
</style></head><body>
<header><span class="logo" onclick="go('home')">AIVT</span><nav><button onclick="go('dash')">Dashboard</button></nav></header>
<main id="app"></main>
<script>
const $=s=>document.querySelector(s), app=$("#app");
const esc=s=>s.replace(/[&<>]/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;"}[c]));
const rx=s=>s.replace(/[.*+?^${}()|[\]\\]/g,"\\$&");
let history=[{t:"21 Oct 2026, 10:12",s:33},{t:"24 Oct 2026, 16:40",s:33}];
function go(v,d){window.scrollTo(0,0);({home,dash,results})[v](d)}
function home(){app.innerHTML=`<h1>Ask an AI for “best financial service in Noida.” Is your name in the answer?</h1>
<p class="lead">AIVT asks AI assistants the questions your customers ask, then scores how often your brand gets named, from 0 to 100.</p>
<button class="btn" onclick="go('dash')">Check your brand</button>
<p class="note">Demo with sample data. The real app runs on Appwrite and Gemini.</p>`}
function dash(){app.innerHTML=`<h1 style="font-size:36px">Check visibility</h1>
<form id="f"><label>Brand name<input id="b" required value="HDB Financial Services"></label>
<label>Category<input id="c" required value="financial service in Noida"></label>
<label>Website<input id="w" type="url" value="https://www.hdbfs.com"></label>
<button class="btn" id="go">Check Visibility Now</button></form>`;
$("#f").onsubmit=e=>{e.preventDefault();const b=$("#b").value.trim(),c=$("#c").value.trim(),g=$("#go");
g.disabled=true;g.textContent="Asking the AI…";setTimeout(()=>go("results",{b,c}),1400)}}
function results({b,c}){
 const R=[
  {q:`best ${c}`,t:`For ${c}, popular options include ${b}, which offers loans and deposits, along with Bajaj Finserv and Muthoot Finance. Compare interest rates and reviews before choosing.`},
  {q:`top 10 ${c}`,t:`1. Bajaj Finserv\n2. Muthoot Finance\n3. Tata Capital\n4. Aditya Birla Capital\n5. Piramal Finance\n6. Hero FinCorp\n(and others, depending on your needs)`},
  {q:`${c} near me`,t:`Nearby providers in this area include ${b} (Sector 18 branch), Tata Capital and Hero FinCorp. Check Google Maps for current hours.`}];
 R.forEach(r=>r.v=r.t.toLowerCase().includes(b.toLowerCase()));
 const found=R.filter(r=>r.v).length,score=Math.round(found/3*100);
 const C=2*Math.PI*54;
 const hl=t=>esc(t).replace(new RegExp("("+rx(esc(b))+")","gi"),"<mark>$1</mark>");
 app.innerHTML=`<h1 style="font-size:36px;margin-bottom:4px">${esc(b)}</h1><div style="opacity:.7">${esc(c)}</div>
 <div class="top"><svg viewBox="0 0 128 128" width="176" height="176" role="img" aria-label="Visibility score ${score} of 100">
 <circle cx="64" cy="64" r="54" fill="none" stroke="currentColor" stroke-opacity=".15" stroke-width="12"/>
 <circle class="ring" id="ring" cx="64" cy="64" r="54" fill="none" stroke="var(--sea)" stroke-width="12" stroke-linecap="round" stroke-dasharray="${C}" stroke-dashoffset="${C}" transform="rotate(-90 64 64)"/>
 <text x="64" y="73" text-anchor="middle" font-family="Fraunces,serif" font-weight="800" font-size="30" fill="currentColor">${score}</text></svg>
 <div class="scroll"><table><tr><th>Platform</th><th>Found</th><th>Score</th></tr>
 <tr><td>Gemini</td><td>${found}/3</td><td><b>${score}</b></td></tr>
 <tr style="opacity:.5"><td>ChatGPT</td><td colspan="2">not connected in demo</td></tr>
 <tr style="opacity:.5"><td>Perplexity</td><td colspan="2">not connected in demo</td></tr></table></div></div>
 <h2>AI responses</h2>${R.map(r=>`<details open><summary>${esc(r.q)} <span class="${r.v?"ok":"no"}">— ${r.v?"mentioned":"not mentioned"}</span></summary><p>${hl(r.t)}</p></details>`).join("")}
 <h2>History</h2><ul>${[{t:"Just now",s:score},...history.slice().reverse()].map(h=>`<li><span>${h.t}</span><b>${h.s}</b></li>`).join("")}</ul>
 <button class="btn" onclick="go('dash')">Check another brand</button>
 <p class="note">Sample data for demo. Real results come from live Gemini answers.</p>`;
 requestAnimationFrame(()=>requestAnimationFrame(()=>$("#ring").style.strokeDashoffset=C*(1-score/100)));
}
home();
</script></body></html>
