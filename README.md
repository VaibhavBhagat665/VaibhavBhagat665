<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Vaibhav Bhagat, terminal</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#0A141B;--panel:#0F1D27;--line:#1F3A4A;--txt:#DCE7EE;--mut:#7C93A3;--amb:#FFB347;--teal:#5ED3C0;--red:#FF6B6B;
 --mono:'JetBrains Mono',SFMono-Regular,Menlo,Consolas,'DejaVu Sans Mono',monospace;
 box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:light){:root:not([data-theme="dark"]){--bg:#F3F6F8;--panel:#FFFFFF;--line:#CBD8E0;--txt:#12232E;--mut:#5B7282;--amb:#B86E00;--teal:#0B7F6E;--red:#C73C3C}}
:root[data-theme="light"]{--bg:#F3F6F8;--panel:#FFFFFF;--line:#CBD8E0;--txt:#12232E;--mut:#5B7282;--amb:#B86E00;--teal:#0B7F6E;--red:#C73C3C}
*{box-sizing:border-box}
html,body{height:100%;margin:0}
body{background:var(--bg);color:var(--txt);font:15px/1.65 var(--mono);display:flex;justify-content:center}
.win{width:100%;max-width:880px;height:100%;display:flex;flex-direction:column;background:var(--panel);border-inline:1px solid var(--line)}
.bar{display:flex;align-items:center;gap:8px;padding:12px 16px;border-bottom:1px solid var(--line);color:var(--mut);font-size:13px}
.bar i{width:11px;height:11px;border-radius:50%;display:block}
.bar span{margin-left:auto;margin-right:auto;transform:translateX(-18px)}
#screen{flex:1;overflow-y:auto;padding:18px 18px 8px;cursor:text}
.ln{white-space:pre-wrap;word-break:break-word;min-height:1.65em}
.p{color:var(--teal)}.d{color:var(--mut)}.a{color:var(--amb)}.r{color:var(--red)}.b{font-weight:700}
.name{font-size:clamp(26px,6vw,44px);font-weight:700;letter-spacing:-1px;line-height:1.1;margin:4px 0 6px}
a{color:var(--teal);text-underline-offset:3px}
a:hover{color:var(--amb)}
[data-cmd]{color:var(--amb);cursor:pointer;border-bottom:1px dashed currentColor}
.card{border:1px solid var(--line);border-left:3px solid var(--amb);border-radius:8px;padding:10px 14px;margin:8px 0;background:var(--bg)}
.tag{display:inline-block;border:1px solid var(--line);border-radius:99px;padding:0 9px;margin:2px 4px 2px 0;font-size:12px;color:var(--mut)}
.tag.h{color:var(--teal);border-color:var(--teal)}
.row{display:grid;grid-template-columns:120px 1fr;gap:4px 12px}
@media(max-width:560px){.row{grid-template-columns:1fr}.row .a{margin-top:6px}}
.chips{display:flex;gap:8px;overflow-x:auto;padding:8px 16px;border-top:1px solid var(--line)}
.chips button{flex:none;font:inherit;font-size:13px;color:var(--txt);background:transparent;border:1px solid var(--line);border-radius:99px;padding:4px 12px;cursor:pointer}
.chips button:hover,.chips button:focus-visible{border-color:var(--amb);color:var(--amb);outline:none}
form{display:flex;gap:8px;padding:10px 16px 14px;align-items:center}
form label{color:var(--teal);white-space:nowrap}
#in{flex:1;min-width:0;background:transparent;border:0;outline:0;color:var(--txt);font:inherit;caret-color:var(--amb)}
@media(max-width:560px){form label span{display:none}}
.bar-fill{color:var(--teal)}
@media(prefers-reduced-motion:reduce){*{scroll-behavior:auto!important}}
</style>
</head>
<body>
<div class="win">
  <div class="bar"><i style="background:var(--red)"></i><i style="background:var(--amb)"></i><i style="background:var(--teal)"></i><span>vaibhav@iiit-sonepat: ~</span></div>
  <div id="screen" aria-live="polite"></div>
  <div class="chips" id="chips"></div>
  <form id="f" autocomplete="off"><label for="in">visitor<span>@vaibhav</span>:~$</label><input id="in" type="text" spellcheck="false" autocapitalize="none" autocorrect="off" aria-label="Type a command"></form>
</div>
<script>
const $=s=>document.querySelector(s), scr=$('#screen'), inp=$('#in');
const reduce=matchMedia('(prefers-reduced-motion: reduce)').matches;
const sleep=ms=>reduce?Promise.resolve():new Promise(r=>setTimeout(r,ms));
const c=n=>`<span data-cmd="${n}">${n}</span>`;
const L=(h,cls='')=>{const d=document.createElement('div');d.className='ln '+cls;d.innerHTML=h;scr.appendChild(d);scr.scrollTop=scr.scrollHeight;return d};
const link=(u,t)=>`<a href="${u}" target="_blank" rel="noopener noreferrer">${t||u}</a>`;
const tags=(a,h)=>a.map(t=>`<span class="tag ${h?'h':''}">${t}</span>`).join('');
const P={
 'sentinel-sec':{u:'https://github.com/VaibhavBhagat665/sentinel-sec',d:'Security agent. Finds vulnerabilities in Python and JavaScript, fixes them, then verifies each fix with Bandit and Semgrep. Runs offline on Ollama or fast on Groq.',t:['Python','Llama 3','Bandit','Semgrep'],x:'<span class="a">$</span> pip install sentinel-sec  '+'<span class="d">(published on PyPI)</span>'},
 'nextalk':{u:'https://github.com/VaibhavBhagat665/nextalk',d:'Real-time team chat with AI summaries and action items, file sharing, and voice and video calls. Web and mobile.',t:['Next.js','TypeScript','Prisma','WebSockets','Docker','Groq']},
 'DistributedKV':{u:'https://github.com/VaibhavBhagat665/Distributed-In-Memory-KV-Store',d:'Distributed in-memory key-value store with Raft consensus and persistence. C++ storage engine under a Go coordination layer.',t:['C++17','Go','Raft']}
};
const C={
 help(){L('<span class="b">Commands</span>');
  [['about','who I am, as a model card'],['projects','things I built'],['stack','tools I use'],['contact','get in touch'],['train','watch a training run'],['theme','toggle light or dark'],['clear','wipe the screen']].forEach(([k,v])=>L(`<div class="row"><span>${c(k)}</span><span class="d">${v}</span></div>`));
  L('<span class="d">Tab completes. Up arrow repeats. Try opening a project by name, e.g. </span>'+`<span data-cmd="open sentinel-sec">open sentinel-sec</span>`)},
 about(){[['name','Vaibhav Bhagat'],['architecture','B.Tech, Information Technology'],['institution','IIIT Sonepat'],['training_ends','2028'],['pretrained_on','Python, C++, Go, TypeScript, Java, Dart'],['fine_tuned_for','transformers and RAG · LLM agents that do real work · full-stack web and mobile apps'],['ask_me_about','RAG pipelines, Raft consensus, Dockerizing a Next.js app'],['open_to','internships, research collaborations, open source']]
  .forEach(([k,v])=>L(`<div class="row"><span class="p">${k}</span><span>${v}</span></div>`));
  L('<span class="d">Next: </span>'+c('projects')+'<span class="d"> or </span>'+c('contact'))},
 projects(){Object.entries(P).forEach(([n,p])=>{L(`<div class="card"><div class="b a">${n}</div><div>${p.d}</div>${p.x?`<div style="margin:6px 0">${p.x}</div>`:''}<div>${tags(p.t)}</div><div style="margin-top:4px">${link(p.u,'view on GitHub')}</div></div>`)});
  L('<span class="d">Tip: </span>'+'<span data-cmd="open sentinel-sec">open sentinel-sec</span>')},
 open(a){const k=Object.keys(P).find(n=>n.toLowerCase()===(a||'').toLowerCase());
  if(!k)return L(`<span class="r">open: pick one of</span> ${Object.keys(P).map(n=>`<span data-cmd="open ${n}">${n}</span>`).join(', ')}`);
  L(`Opening ${link(P[k].u,k)} <span class="d">(tap the link if a new tab was blocked)</span>`)},
 stack(){[['languages',['Python','C++','C','Go','Java','TypeScript','JavaScript','Dart','HTML','CSS'],0],['ai / ml',['PyTorch','TensorFlow','scikit-learn','NumPy','Pandas','Keras','LangChain','Streamlit','Groq','Ollama'],1],['web + mobile',['React','Next.js','Node.js','Express','Tailwind','Vite','Firebase','MongoDB'],0],['shipping',['Git','GitHub','Docker','Vercel','Google Cloud'],0]]
  .forEach(([k,a,h])=>L(`<div class="row"><span class="a">${k}</span><span>${tags(a,h&&0)}</span></div>`))},
 contact(){L('Open to internships, research collaborations and open source.');
  L(`<div class="row"><span class="a">email</span>${link('mailto:vaibhavbhagat7461@gmail.com','vaibhavbhagat7461@gmail.com')}</div>`);
  L(`<div class="row"><span class="a">linkedin</span>${link('https://linkedin.com/in/vaibhavbhagat5','linkedin.com/in/vaibhavbhagat5')}</div>`);
  L(`<div class="row"><span class="a">github</span>${link('https://github.com/VaibhavBhagat665','github.com/VaibhavBhagat665')}</div>`);
  L(`<div class="row"><span class="a">pypi</span>${link('https://pypi.org/project/sentinel-sec/','pypi.org/project/sentinel-sec')}</div>`)},
 async train(){const g=Math.random;let l=1;
  for(let e=1;e<=12;e++){l=0.12+0.88*Math.exp(-0.38*e)+g()*0.02;const n=Math.round((1-l)*28);
   L(`epoch ${String(e).padStart(2)}/12  <span class="bar-fill">${'█'.repeat(n)}</span><span class="d">${'░'.repeat(28-n)}</span>  loss ${l.toFixed(3)}`);await sleep(260)}
  L('<span class="p">converged.</span> <span class="d">Still training in real life, though. Check back for updates.</span>')},
 theme(){const r=document.documentElement,dark=getComputedStyle(r).getPropertyValue('--bg').trim()==='#0A141B';r.dataset.theme=dark?'light':'dark';L(`<span class="d">theme: ${dark?'light':'dark'}</span>`)},
 clear(){scr.innerHTML=''},
 whoami(){L('visitor. Welcome, you can call '+c('about')+' to meet the owner.')},
 ls(){L('<span class="a">sentinel-sec/</span>  <span class="a">nextalk/</span>  <span class="a">DistributedKV/</span>  about.yaml')},
 sudo(a){L(a==='hire-me'?'<span class="p">[sudo] permission granted.</span> Drafting offer letter... just kidding, '+link('mailto:vaibhavbhagat7461@gmail.com?subject=Hello%20Vaibhav','send an email')+' instead.':'<span class="r">visitor is not in the sudoers file. This incident will be reported.</span>')}
};
const names=Object.keys(C);const hist=[];let hi=0;
async function run(raw){const v=raw.trim();L(`<span class="p">visitor@vaibhav:~$</span> ${v.replace(/</g,'&lt;')}`);if(!v)return;
 hist.push(v);hi=hist.length;const [cmd,...rest]=v.split(/\s+/),arg=rest.join(' ');
 if(C[cmd])await C[cmd](arg);else L(`<span class="r">command not found:</span> ${cmd.replace(/</g,'&lt;')}. Type ${c('help')}.`);
 scr.scrollTop=scr.scrollHeight}
$('#f').addEventListener('submit',e=>{e.preventDefault();const v=inp.value;inp.value='';run(v)});
inp.addEventListener('keydown',e=>{
 if(e.key==='ArrowUp'&&hi>0){e.preventDefault();inp.value=hist[--hi]}
 else if(e.key==='ArrowDown'){e.preventDefault();hi=Math.min(hi+1,hist.length);inp.value=hist[hi]||''}
 else if(e.key==='Tab'){e.preventDefault();const m=names.filter(n=>n.startsWith(inp.value));if(m.length===1)inp.value=m[0]}});
document.addEventListener('click',e=>{const t=e.target.closest('[data-cmd]');if(t){run(t.dataset.cmd);if(matchMedia('(pointer:fine)').matches)inp.focus();return}
 if(!e.target.closest('a,button')&&getSelection().toString()==='')inp.focus()});
$('#chips').innerHTML=['about','projects','stack','contact','train','help'].map(n=>`<button type="button" data-cmd="${n}">${n}</button>`).join('');
(async()=>{
 L('<div class="name">Vaibhav Bhagat</div>');
 L('AI/ML student building models and the full-stack apps around them.','d');
 await sleep(350);
 for(const t of ['loading model_card.yaml','mounting projects/','warming up the loss curve']){L(`<span class="p">ok</span>  ${t}`);await sleep(320)}
 L('');L('Type '+c('help')+', or tap a command below. Start with '+c('about')+'.');
 if(matchMedia('(pointer:fine)').matches)inp.focus();
})();
</script>
</body>
</html>
