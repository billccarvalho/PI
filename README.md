<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Diagnóstico de Processo · Recuperação Química</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;}
body{background:#07090d;color:#cbd5e1;font-family:'Inter',system-ui,sans-serif;min-height:100vh;}
header{background:#0c1018;border-bottom:1px solid #1a2535;padding:14px 24px 10px;position:sticky;top:0;z-index:100;}
.title{font-size:11px;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:#00c9a7;margin-bottom:2px;}
.sub{font-size:11px;color:#4a6180;}
.tabs{display:flex;background:#0c1018;border-bottom:1px solid #1a2535;}
.tab{flex:1;padding:11px 4px;font-size:11px;font-weight:700;border:none;cursor:pointer;background:transparent;color:#4a6180;letter-spacing:.04em;border-bottom:2px solid transparent;transition:all .2s;}
.tab.active{color:#00c9a7;border-bottom-color:#00c9a7;}
.screen{display:none;padding:16px 20px 80px;}
.screen.active{display:block;}
label.lbl{display:block;font-size:11px;font-weight:600;letter-spacing:.1em;text-transform:uppercase;color:#4a6180;margin-bottom:7px;margin-top:16px;}
input[type=text],textarea,select{width:100%;background:#111720;border:1px solid #1a2535;border-radius:8px;color:#cbd5e1;font-family:monospace;font-size:13px;padding:11px 13px;outline:none;resize:none;}
input[type=text]:focus,textarea:focus{border-color:#00c9a7;}
.hint{font-size:11px;color:#4a6180;margin-top:6px;line-height:1.6;}
.chips{display:flex;flex-wrap:wrap;gap:5px;margin-top:4px;}
.chip{padding:6px 13px;border-radius:20px;font-size:11px;font-weight:600;cursor:pointer;border:1px solid #1a2535;background:transparent;color:#4a6180;transition:all .15s;}
.chip.at{border-color:#00c9a7;background:#00c9a712;color:#00c9a7;}
.chip.ab{border-color:#0ea5e9;background:#0ea5e915;color:#0ea5e9;}
.btn{border:none;border-radius:8px;font-weight:700;font-size:13px;letter-spacing:.06em;padding:13px 20px;cursor:pointer;text-transform:uppercase;width:100%;margin-top:14px;}
.btn-teal{background:#00c9a7;color:#07090d;}
.btn-blue{background:#0ea5e9;color:#fff;}
.btn-ai{background:linear-gradient(135deg,#00c9a7,#0ea5e9);color:#07090d;border:none;border-radius:8px;font-weight:700;font-size:13px;padding:14px 20px;width:100%;cursor:pointer;text-transform:uppercase;margin-top:12px;letter-spacing:.06em;}
.btn-ghost{background:transparent;border:1px solid #1a2535;color:#4a6180;font-size:12px;padding:8px 14px;border-radius:8px;cursor:pointer;font-weight:600;}
.card{background:#111720;border:1px solid #1a2535;border-radius:10px;margin-bottom:10px;overflow:hidden;}
.card.open{border-color:#0ea5e9;}
.card-h{padding:12px 15px;display:flex;align-items:center;justify-content:space-between;cursor:pointer;}
.error{background:#ef444415;border:1px solid #ef444444;border-radius:8px;color:#ef4444;font-size:12px;padding:10px 14px;margin-top:10px;line-height:1.6;}
.stat-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:6px;}
.sb{background:#07090d;border-radius:6px;padding:8px 10px;}
.sk{font-size:9px;color:#4a6180;margin-bottom:2px;}
.sv{font-size:14px;font-weight:700;}
.summary{background:#0c1018;border-bottom:1px solid #1a2535;padding:10px 20px;display:flex;justify-content:space-between;align-items:center;}
.ai-box{background:#111720;border:1px solid #00c9a744;border-radius:10px;padding:16px;margin-bottom:12px;}
.ai-hdr{font-size:10px;font-weight:700;letter-spacing:.12em;text-transform:uppercase;color:#00c9a7;margin-bottom:12px;}
.ai-body{font-size:13px;line-height:1.9;color:#cbd5e1;white-space:pre-wrap;}
.spinner{width:44px;height:44px;border:3px solid #1a2535;border-top:3px solid #00c9a7;border-radius:50%;margin:0 auto 20px;animation:spin .8s linear infinite;}
@keyframes spin{to{transform:rotate(360deg)}}
.lc{text-align:center;padding:40px 0;}
.drop-zone{border:2px dashed #1a2535;border-radius:10px;padding:28px;text-align:center;cursor:pointer;transition:border-color .2s;margin-top:10px;}
.drop-zone:hover{border-color:#00c9a7;}
.mode-toggle{display:flex;gap:8px;margin-bottom:4px;}
.mode-btn{flex:1;padding:10px;border-radius:8px;font-size:12px;font-weight:700;cursor:pointer;border:1px solid #1a2535;background:transparent;color:#4a6180;transition:all .2s;}
.mode-btn.active{border-color:#00c9a7;background:#00c9a712;color:#00c9a7;}
.prog-bar-bg{background:#1a2535;border-radius:4px;height:4px;width:80%;margin:0 auto 20px;}
.prog-bar{background:#00c9a7;border-radius:4px;height:4px;width:0%;transition:width .3s;}
.divider{border:none;border-top:1px solid #1a2535;margin:20px 0;}
</style>
</head>
<body>

<header>
  <div class="title">⬡ Diagnóstico de Processo · Recuperação Química</div>
  <div class="sub" id="hdr-sub">Klabin Otacílio Costa · otapi.klabin.com.br</div>
</header>

<!-- INPUT -->
<div id="screen-input" class="screen active">

  <!-- Modo -->
  <div style="margin-bottom:4px;margin-top:4px;">
    <div class="mode-toggle">
      <button class="mode-btn active" id="mb-pi" onclick="setMode('pi')">📡 Buscar no PI</button>
      <button class="mode-btn" id="mb-csv" onclick="setMode('csv')">📂 Abrir CSV</button>
    </div>
    <div class="hint">Modo PI: conecta direto ao servidor (use dentro da rede Klabin). Modo CSV: cole ou abra um arquivo exportado.</div>
  </div>

  <!-- MODO PI -->
  <div id="mode-pi">
    <label class="lbl">Tag base (variável alvo)</label>
    <input type="text" id="pi-base" placeholder="ex: 524AT0893D_MV" autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false"/>
    <div class="hint">O app calcula a correlação de todas as tags do prefixo contra esta.</div>

    <label class="lbl">Prefixo das tags a comparar</label>
    <div class="chips" id="prefix-chips">
      <button class="chip at" onclick="setPrefix('524*')">524*</button>
      <button class="chip" onclick="setPrefix('514*')">514*</button>
      <button class="chip" onclick="setPrefix('515*')">515*</button>
      <button class="chip" onclick="setPrefix('513*')">513*</button>
      <button class="chip" onclick="setPrefix('5*')">5*</button>
    </div>
    <input type="text" id="pi-prefix" value="524*" style="margin-top:7px;" autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false" oninput="syncPrefix(this.value)"/>

    <label class="lbl">Período histórico</label>
    <div class="chips" id="period-chips">
      <button class="chip" onclick="setPeriod('*-8h')">8h</button>
      <button class="chip ab" onclick="setPeriod('*-24h')">24h</button>
      <button class="chip" onclick="setPeriod('*-48h')">48h</button>
      <button class="chip" onclick="setPeriod('*-7d')">7 dias</button>
      <button class="chip" onclick="setPeriod('*-30d')">30 dias</button>
    </div>

    <div id="pi-error" class="error" style="display:none"></div>
    <button class="btn btn-teal" onclick="handlePI()">Conectar ao PI e calcular</button>
    <div class="hint" style="text-align:center;margin-top:8px;">Autenticação Windows automática · Use dentro da rede Klabin</div>
  </div>

  <!-- MODO CSV -->
  <div id="mode-csv" style="display:none;">
    <label class="lbl">Cole os dados aqui</label>
    <textarea id="csv-raw" rows="7" placeholder="Timestamp&#9;524AT0893D_MV&#9;524PDT0020_MV&#10;2025-01-01 00:00&#9;65.2&#9;19.8&#10;..."></textarea>
    <div class="drop-zone" onclick="document.getElementById('file-inp').click()">
      <div style="font-size:22px;">📂</div>
      <p style="font-size:12px;color:#4a6180;margin-top:6px;">Arraste CSV / TXT ou toque para abrir</p>
      <input type="file" id="file-inp" accept=".csv,.txt,.tsv" style="display:none" onchange="readFile(this.files[0])"/>
    </div>
    <div id="csv-error" class="error" style="display:none"></div>
    <button class="btn btn-teal" onclick="handleCSV()">Analisar dados</button>
  </div>
</div>

<!-- LOADING -->
<div id="screen-loading" class="screen" style="display:none;">
  <div class="lc">
    <div class="spinner"></div>
    <div style="font-size:13px;color:#00c9a7;font-weight:600;" id="prog-msg">Conectando...</div>
    <div style="font-size:11px;color:#4a6180;margin-top:4px;" id="prog-sub"></div>
    <div class="prog-bar-bg" style="margin-top:16px;"><div class="prog-bar" id="prog-bar"></div></div>
    <button class="btn-ghost" style="margin-top:20px;" onclick="abortRun()">Cancelar</button>
  </div>
</div>

<!-- TABS -->
<div id="tabs-bar" style="display:none;">
  <div class="tabs">
    <button class="tab active" onclick="showTab(0)" id="tab0">Estatísticas</button>
    <button class="tab" onclick="showTab(1)" id="tab1">Correlações</button>
    <button class="tab" onclick="showTab(2)" id="tab2">Boxplot</button>
    <button class="tab" onclick="showTab(3)" id="tab3">IA</button>
  </div>
</div>

<div id="summary-bar" class="summary" style="display:none;">
  <div>
    <div id="sum-main" style="font-size:13px;font-weight:700;color:#00c9a7;"></div>
    <div id="sum-sub" style="font-size:11px;color:#4a6180;"></div>
  </div>
  <button class="btn-ghost" onclick="resetAll()">← Novo</button>
</div>

<!-- ESTATÍSTICAS -->
<div id="screen-stats" class="screen" style="display:none;"></div>

<!-- CORRELAÇÕES -->
<div id="screen-corr" class="screen" style="display:none;">
  <div class="chips" style="margin-bottom:12px;">
    <button class="chip ab" id="cf0" onclick="setFilter(0)">Todos</button>
    <button class="chip" id="cf4" onclick="setFilter(0.4)">≥ 0.4</button>
    <button class="chip" id="cf6" onclick="setFilter(0.6)">≥ 0.6</button>
    <button class="chip" id="cf8" onclick="setFilter(0.8)">≥ 0.8</button>
  </div>
  <div id="corr-cards"></div>
</div>

<!-- BOXPLOT -->
<div id="screen-box" class="screen" style="display:none;">
  <label class="lbl" style="margin-top:0;">Variáveis</label>
  <div id="box-chips" class="chips" style="margin-bottom:14px;"></div>
  <div id="box-charts"></div>
</div>

<!-- IA -->
<div id="screen-ai" class="screen" style="display:none;">
  <div id="ai-form">
    <label class="lbl" style="margin-top:0;">Contexto do problema</label>
    <textarea id="ai-ctx" rows="3" placeholder="Ex: Investigando arraste no EV3, CO alto na caldeira CR4..."></textarea>
    <div class="hint">A IA analisa todas as correlações + estatísticas e entrega diagnóstico técnico com mecanismos físico-químicos e ações recomendadas.</div>
    <button class="btn-ai" onclick="handleAI()">✦ Gerar diagnóstico com IA</button>
  </div>
  <div id="ai-loading" class="lc" style="display:none;">
    <div class="spinner"></div>
    <div style="font-size:13px;color:#00c9a7;font-weight:600;">Analisando com IA...</div>
    <div style="font-size:11px;color:#4a6180;margin-top:4px;" id="ai-prog"></div>
  </div>
  <div id="ai-out" style="display:none;">
    <div class="ai-box">
      <div class="ai-hdr">✦ Diagnóstico · Claude AI</div>
      <div class="ai-body" id="ai-text"></div>
    </div>
    <button class="btn-ghost" style="width:100%;" onclick="resetAI()">↺ Novo diagnóstico</button>
  </div>
</div>

<script>
// ── Config PI ──
const PI_BASE = 'https://otapi.klabin.com.br/piwebapi';
const SERVERS = [
  {name:'otasrv048-vm', webId:'F1DSng4U9O8oTU2FzphTAzCMGQT1RBU1JwMDQ4LVZN'},
  {name:'CPSRV009',     webId:'F1DSAAAAAAAAAAAD____2xVC2QQ1BTU1YwMDk'},
  {name:'MASRV011',     webId:'F1DSAAAAAAAAAAAACFAuDwTUFTU1YwMTE'},
];

// ── State ──
let DATA=null, CORRS=[], STATS={}, corrFilter=0, openCard=null;
let boxSelected=new Set(), aborted=false;
let selMode='pi', selPrefix='524*', selPeriod='*-24h';
let baseMeta=null;

// ── Mode ──
function setMode(m) {
  selMode=m;
  document.getElementById('mode-pi').style.display=m==='pi'?'block':'none';
  document.getElementById('mode-csv').style.display=m==='csv'?'block':'none';
  document.getElementById('mb-pi').classList.toggle('active',m==='pi');
  document.getElementById('mb-csv').classList.toggle('active',m==='csv');
}
function setPrefix(p) {
  selPrefix=p;
  document.getElementById('pi-prefix').value=p;
  document.querySelectorAll('#prefix-chips .chip').forEach(b=>{
    b.classList.toggle('at',b.textContent===p);
  });
}
function syncPrefix(v){selPrefix=v;}
function setPeriod(p) {
  selPeriod=p;
  document.querySelectorAll('#period-chips .chip').forEach(b=>{
    b.classList.toggle('ab',b.getAttribute('onclick').includes("'"+p+"'"));
  });
}

// ── File ──
function readFile(file) {
  if(!file)return;
  const r=new FileReader();
  r.onload=e=>{document.getElementById('csv-raw').value=e.target.result;};
  r.readAsText(file,'latin1');
}
const dz=document.querySelector('.drop-zone');
dz.addEventListener('dragover',e=>{e.preventDefault();dz.style.borderColor='#00c9a7';});
dz.addEventListener('dragleave',()=>{dz.style.borderColor='';});
dz.addEventListener('drop',e=>{e.preventDefault();dz.style.borderColor='';if(e.dataTransfer.files[0])readFile(e.dataTransfer.files[0]);});

// ── Parse ──
function parseCSV(raw) {
  const lines=raw.trim().split(/\r?\n/).filter(Boolean);
  if(lines.length<3)throw new Error('Mínimo 3 linhas.');
  const sep=lines[0].includes('\t')?'\t':lines[0].includes(';')?';':',';
  const headers=lines[0].split(sep).map(h=>h.trim().replace(/^"|"$/g,''));
  const rows=lines.slice(1).map(line=>{
    const cols=line.split(sep).map(c=>c.trim().replace(/^"|"$/g,'').replace(',','.'));
    const row={};
    headers.forEach((h,j)=>{const v=parseFloat(cols[j]);row[h]=isNaN(v)?cols[j]:v;});
    return row;
  });
  const numericCols=headers.filter(h=>rows.filter(r=>typeof r[h]==='number').length>=3);
  if(numericCols.length<2)throw new Error('Precisa de pelo menos 2 colunas numéricas.');
  return {headers,numericCols,rows};
}

// ── Stats & Pearson ──
function computeStats(rows,cols) {
  const s={};
  cols.forEach(col=>{
    const vals=rows.map(r=>r[col]).filter(v=>typeof v==='number').sort((a,b)=>a-b);
    const n=vals.length;
    const mean=vals.reduce((a,b)=>a+b,0)/n;
    const std=Math.sqrt(vals.reduce((a,b)=>a+(b-mean)**2,0)/n);
    const q1=vals[Math.floor(n*0.25)],med=vals[Math.floor(n*0.5)],q3=vals[Math.floor(n*0.75)];
    const iqr=q3-q1;
    s[col]={n,mean,std,min:vals[0],max:vals[n-1],q1,med,q3,iqr,
      lo:q1-1.5*iqr,hi:q3+1.5*iqr,
      outliers:vals.filter(v=>v<q1-1.5*iqr||v>q3+1.5*iqr)};
  });
  return s;
}
function pearson(xs,ys){
  const n=xs.length;if(n<3)return null;
  const mx=xs.reduce((a,b)=>a+b,0)/n,my=ys.reduce((a,b)=>a+b,0)/n;
  let num=0,dx2=0,dy2=0;
  for(let i=0;i<n;i++){num+=(xs[i]-mx)*(ys[i]-my);dx2+=(xs[i]-mx)**2;dy2+=(ys[i]-my)**2;}
  const d=Math.sqrt(dx2*dy2);return d===0?0:num/d;
}
function alignSeries(a,b){
  if(!a.length||!b.length)return{xs:[],ys:[]};
  const[base,other]=a.length<=b.length?[a,b]:[b,a];
  const swapped=a.length>b.length;
  const xs=[],ys=[];
  for(const pt of base){
    const cl=other.reduce((p,c)=>Math.abs(c.t-pt.t)<Math.abs(p.t-pt.t)?c:p);
    if(Math.abs(cl.t-pt.t)<3600000){
      if(!swapped){xs.push(pt.v);ys.push(cl.v);}else{xs.push(cl.v);ys.push(pt.v);}
    }
  }
  return{xs,ys};
}
function computeCorrelations(rows,cols){
  const pairs=[];
  for(let i=0;i<cols.length;i++)for(let j=i+1;j<cols.length;j++){
    const h1=cols[i],h2=cols[j];
    const f=rows.filter(r=>typeof r[h1]==='number'&&typeof r[h2]==='number');
    if(f.length<3)continue;
    const xs=f.map(r=>r[h1]),ys=f.map(r=>r[h2]);
    const r=pearson(xs,ys);
    if(r!==null)pairs.push({h1,h2,r,n:f.length,xs,ys});
  }
  return pairs.sort((a,b)=>Math.abs(b.r)-Math.abs(a.r));
}

// ── PI API ──
async function resolveTag(name){
  for(const srv of SERVERS){
    try{
      const url=PI_BASE+'/dataservers/'+srv.webId+'/points?nameFilter='+encodeURIComponent(name)+'&maxCount=5';
      const r=await fetch(url,{credentials:'include'});
      if(!r.ok)continue;
      const d=await r.json();
      const item=(d.Items||[]).find(i=>i.Name===name)||(d.Items||[])[0];
      if(item&&item.WebId)return{webId:item.WebId,label:item.Descriptor||name,unit:item.EngineeringUnits||'',server:srv.name};
    }catch{}
  }
  throw new Error('Tag não encontrada: '+name);
}
async function fetchAllTags(prefix){
  const all=[],seen=new Set();
  for(const srv of SERVERS){
    try{
      const url=PI_BASE+'/dataservers/'+srv.webId+'/points?nameFilter='+encodeURIComponent(prefix)+'&maxCount=500';
      const r=await fetch(url,{credentials:'include'});
      if(!r.ok)continue;
      const d=await r.json();
      for(const p of(d.Items||[])){
        if(!seen.has(p.Name)){seen.add(p.Name);all.push({tag:p.Name,webId:p.WebId,label:p.Descriptor||p.Name,unit:p.EngineeringUnits||''});}
      }
    }catch{}
  }
  return all;
}
async function getRecorded(webId,startTime){
  const url=PI_BASE+'/streams/'+webId+'/recorded?startTime='+encodeURIComponent(startTime)+'&endTime=*&maxCount=600';
  const r=await fetch(url,{credentials:'include'});
  if(!r.ok)throw new Error('HTTP '+r.status);
  const d=await r.json();
  return(d.Items||[]).filter(i=>i.Good!==false&&typeof i.Value==='number').map(i=>({t:new Date(i.Timestamp).getTime(),v:i.Value}));
}

function setProgress(msg,done,total){
  document.getElementById('prog-msg').textContent=msg;
  document.getElementById('prog-sub').textContent=done+' / '+total;
  document.getElementById('prog-bar').style.width=(total>0?Math.round(done/total*100):0)+'%';
}

// ── PI flow ──
async function handlePI(){
  const baseTag=document.getElementById('pi-base').value.trim();
  const prefix=document.getElementById('pi-prefix').value.trim()||selPrefix;
  const errEl=document.getElementById('pi-error');
  errEl.style.display='none';
  if(!baseTag){errEl.textContent='Digite a tag base.';errEl.style.display='block';return;}
  if(baseTag.includes('*')){errEl.textContent='Tag base não pode conter * — nome exato.';errEl.style.display='block';return;}

  aborted=false;
  showScreen('loading');
  setProgress('Resolvendo tag base...',0,1);

  try{
    const base=await resolveTag(baseTag);
    baseMeta={tag:baseTag,...base};

    setProgress('Buscando tags '+prefix+'...',0,1);
    const allTags=await fetchAllTags(prefix);
    if(!allTags.length)throw new Error('Nenhuma tag encontrada com filtro "'+prefix+'".');

    const candidates=allTags.filter(t=>t.tag!==baseTag);
    setProgress('Carregando tag base...',0,candidates.length);
    const baseData=await getRecorded(base.webId,selPeriod);
    if(baseData.length<3)throw new Error('Tag base sem dados suficientes no período.');

    // Busca dados de cada candidato e calcula correlação
    const pairs=[];
    const rows=[];  // para estatísticas: formato {tag: valor por timestamp}

    for(let i=0;i<candidates.length;i++){
      if(aborted)return;
      const c=candidates[i];
      setProgress('Calculando: '+c.tag,i+1,candidates.length);
      try{
        const data=await getRecorded(c.webId,selPeriod);
        if(data.length<3)continue;
        const{xs,ys}=alignSeries(baseData,data);
        if(xs.length<3)continue;
        const r=pearson(xs,ys);
        if(r!==null)pairs.push({h1:baseTag,h2:c.tag,label:c.label,r,n:xs.length,xs,ys});
      }catch{}
    }

    // Monta DATA sintético para estatísticas e boxplot
    const allCols=[baseTag,...candidates.map(c=>c.tag)];
    const allData=[{webId:base.webId,data:baseData}];
    // Usa xs/ys dos pares para stats
    const colData={};
    colData[baseTag]=baseData.map(p=>p.v);
    pairs.forEach(p=>{colData[p.h2]=p.ys;});

    const syntheticRows=baseData.map((pt,i)=>{
      const row={Timestamp:new Date(pt.t).toISOString()};
      row[baseTag]=pt.v;
      pairs.forEach(p=>{if(p.xs[i]!==undefined)row[p.h2]=p.ys[i];});
      return row;
    });

    const numericCols=[baseTag,...pairs.map(p=>p.h2)];
    DATA={headers:['Timestamp',...numericCols],numericCols,rows:syntheticRows};
    CORRS=pairs.sort((a,b)=>Math.abs(b.r)-Math.abs(a.r));
    STATS=computeStats(syntheticRows,numericCols);

    finalize(baseTag+' vs '+prefix, candidates.length+' tags analisadas · '+baseData.length+' amostras');
  }catch(e){
    document.getElementById('pi-error').textContent='⚠ '+e.message;
    document.getElementById('pi-error').style.display='block';
    showScreen('input');
  }
}

function abortRun(){aborted=true;showScreen('input');}

// ── CSV flow ──
function handleCSV(){
  const raw=document.getElementById('csv-raw').value;
  const errEl=document.getElementById('csv-error');
  errEl.style.display='none';
  try{
    DATA=parseCSV(raw);
    CORRS=computeCorrelations(DATA.rows,DATA.numericCols);
    STATS=computeStats(DATA.rows,DATA.numericCols);
    baseMeta=null;
    finalize(DATA.numericCols.length+' variáveis · '+CORRS.length+' pares',DATA.rows.length+' amostras');
  }catch(e){errEl.textContent='⚠ '+e.message;errEl.style.display='block';}
}

function finalize(main,sub){
  renderStats();renderCorrelations();renderBoxChips();
  document.getElementById('sum-main').textContent=main;
  document.getElementById('sum-sub').textContent=sub;
  document.getElementById('hdr-sub').textContent=main;
  showScreen('tabs');showTab(0);
}

// ── Screens ──
function showScreen(name){
  ['input','loading'].forEach(id=>{
    document.getElementById('screen-'+id).style.display=name===id?'block':'none';
  });
  const isTabs=name==='tabs';
  document.getElementById('tabs-bar').style.display=isTabs?'block':'none';
  document.getElementById('summary-bar').style.display=isTabs?'flex':'none';
  if(!isTabs){
    ['stats','corr','box','ai'].forEach(id=>{
      document.getElementById('screen-'+id).style.display='none';
    });
  }
}

function resetAll(){
  DATA=null;CORRS=[];STATS={};openCard=null;boxSelected=new Set();
  document.getElementById('csv-raw').value='';
  document.getElementById('ai-form').style.display='block';
  document.getElementById('ai-out').style.display='none';
  document.getElementById('ai-loading').style.display='none';
  document.getElementById('hdr-sub').textContent='Klabin Otacílio Costa · otapi.klabin.com.br';
  showScreen('input');
}

// ── Tabs ──
function showTab(i){
  const ids=['stats','corr','box','ai'];
  ids.forEach((id,j)=>{
    document.getElementById('screen-'+id).style.display=j===i?'block':'none';
    document.getElementById('tab'+j).classList.toggle('active',j===i);
  });
  if(i===2)renderBoxPlots();
}

// ── Stat tab ──
function renderStats(){
  const el=document.getElementById('screen-stats');
  const baseLabel=baseMeta?`<div style="background:#00c9a712;border:1px solid #00c9a744;border-radius:8px;padding:10px 14px;margin-bottom:14px;font-size:12px;color:#00c9a7;font-family:monospace;">⬡ Tag base: <strong>${baseMeta.tag}</strong> — ${baseMeta.label}</div>`:'';
  el.innerHTML=baseLabel+DATA.numericCols.map(col=>{
    const s=STATS[col];
    const f=v=>typeof v==='number'?v.toFixed(3):v;
    const out=s.outliers.length>0?`<div style="margin-top:6px;font-size:11px;color:#f59e0b;">⚠ ${s.outliers.length} outlier(s)</div>`:'';
    return`<div class="card" style="margin-bottom:10px;"><div style="padding:11px 14px;">
      <div style="font-size:12px;font-weight:700;font-family:monospace;color:#cbd5e1;margin-bottom:8px;">${col}</div>
      <div class="stat-grid">
        <div class="sb"><div class="sk">Média</div><div class="sv">${f(s.mean)}</div></div>
        <div class="sb"><div class="sk">Desvio</div><div class="sv">${f(s.std)}</div></div>
        <div class="sb"><div class="sk">n</div><div class="sv">${s.n}</div></div>
        <div class="sb"><div class="sk">Mín</div><div class="sv">${f(s.min)}</div></div>
        <div class="sb"><div class="sk">Mediana</div><div class="sv">${f(s.med)}</div></div>
        <div class="sb"><div class="sk">Máx</div><div class="sv">${f(s.max)}</div></div>
        <div class="sb"><div class="sk">Q1</div><div class="sv">${f(s.q1)}</div></div>
        <div class="sb"><div class="sk">Q3</div><div class="sv">${f(s.q3)}</div></div>
        <div class="sb"><div class="sk">IQR</div><div class="sv">${f(s.iqr)}</div></div>
      </div>${out}
    </div></div>`;
  }).join('');
}

// ── Corr tab ──
function corrColor(r){const a=Math.abs(r);if(a>=0.8)return r>0?'#22c55e':'#ef4444';if(a>=0.6)return'#f59e0b';if(a>=0.4)return'#00c9a7';return'#4a6180';}
function corrLabel(r){const a=Math.abs(r),d=r>0?'▲':'▼';if(a>=0.8)return d+' Forte';if(a>=0.6)return d+' Moderada';if(a>=0.4)return d+' Fraca';return'— Nenhuma';}

function setFilter(v){
  corrFilter=v;
  [['cf0',0],['cf4',.4],['cf6',.6],['cf8',.8]].forEach(([id,val])=>{
    document.getElementById(id).classList.toggle('ab',val===v);
  });
  renderCorrelations();
}

function makeSVG(xs,ys,h1,h2,r){
  const W=300,H=160,P=28;
  const mnX=Math.min(...xs),mxX=Math.max(...xs),mnY=Math.min(...ys),mxY=Math.max(...ys);
  const sx=v=>P+((v-mnX)/(mxX-mnX||1))*(W-P*2);
  const sy=v=>H-P-((v-mnY)/(mxY-mnY||1))*(H-P*2);
  const n=xs.length,mx=xs.reduce((a,b)=>a+b,0)/n,my=ys.reduce((a,b)=>a+b,0)/n;
  let num=0,den=0;xs.forEach((x,i)=>{num+=(x-mx)*(ys[i]-my);den+=(x-mx)**2;});
  const sl=den===0?0:num/den,ic=my-sl*mx;
  const cl=(v,a,b)=>Math.max(a,Math.min(b,v));
  const color=corrColor(r);
  let s=`<svg width="100%" viewBox="0 0 ${W} ${H}" style="display:block;background:#07090d;border-radius:6px;margin-top:10px;">`;
  [.25,.5,.75].forEach(f=>{s+=`<line x1="${P}" y1="${P+(H-P*2)*f}" x2="${W-P}" y2="${P+(H-P*2)*f}" stroke="#1a2535" stroke-width=".5"/>`;});
  s+=`<line x1="${sx(mnX).toFixed(1)}" y1="${sy(cl(sl*mnX+ic,mnY,mxY)).toFixed(1)}" x2="${sx(mxX).toFixed(1)}" y2="${sy(cl(sl*mxX+ic,mnY,mxY)).toFixed(1)}" stroke="${color}" stroke-width="1.5" stroke-dasharray="4,2" opacity=".8"/>`;
  xs.forEach((x,i)=>{s+=`<circle cx="${sx(x).toFixed(1)}" cy="${sy(ys[i]).toFixed(1)}" r="2.5" fill="${color}" opacity=".5"/>`;});
  s+=`<text x="${W/2}" y="${H-4}" text-anchor="middle" font-size="9" fill="#4a6180">${h1.slice(-20)}</text>`;
  s+=`<text x="9" y="${H/2}" text-anchor="middle" font-size="9" fill="#4a6180" transform="rotate(-90,9,${H/2})">${h2.slice(-16)}</text>`;
  s+=`</svg>`;return s;
}

function renderCorrelations(){
  const el=document.getElementById('corr-cards');
  const filtered=CORRS.filter(p=>Math.abs(p.r)>=corrFilter);
  if(!filtered.length){el.innerHTML='<div style="text-align:center;color:#4a6180;padding:40px 0;font-size:13px;">Nenhum par com este filtro.</div>';return;}
  el.innerHTML=filtered.map((p,i)=>{
    const color=corrColor(p.r),lbl=corrLabel(p.r);
    const desc=p.label&&p.label!==p.h2?`<div style="font-size:10px;color:#4a6180;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;">${p.label}</div>`:'';
    return`<div class="card" id="cc${i}">
      <div class="card-h" onclick="toggleCorr(${i})">
        <div style="flex:1;min-width:0;margin-right:10px;">
          <div style="font-size:12px;font-weight:700;font-family:monospace;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;"><span style="color:#4a6180;font-weight:400;">#${i+1} </span>${baseMeta?p.h2:p.h1+' × '+p.h2}</div>
          ${desc}
          <div style="font-size:10px;color:#4a6180;">n=${p.n}</div>
        </div>
        <div style="text-align:right;flex-shrink:0;">
          <div style="font-size:20px;font-weight:800;color:${color};line-height:1;">${p.r.toFixed(3)}</div>
          <div style="font-size:10px;font-weight:700;background:${color}22;color:${color};border-radius:20px;padding:2px 8px;display:inline-block;margin-top:2px;">${lbl}</div>
        </div>
      </div>
      <div id="cb${i}" style="display:none;padding:0 14px 12px;border-top:1px solid #1a2535;">
        ${makeSVG(p.xs,p.ys,p.h1,p.h2,p.r)}
        <div class="stat-grid" style="margin-top:8px;">
          <div class="sb"><div class="sk">R</div><div class="sv">${p.r.toFixed(4)}</div></div>
          <div class="sb"><div class="sk">R²</div><div class="sv">${(p.r*p.r).toFixed(4)}</div></div>
          <div class="sb"><div class="sk">n</div><div class="sv">${p.n}</div></div>
        </div>
      </div>
    </div>`;
  }).join('');
}

function toggleCorr(i){
  const body=document.getElementById('cb'+i),card=document.getElementById('cc'+i);
  const isOpen=body.style.display==='block';
  document.querySelectorAll('[id^="cb"]').forEach(b=>b.style.display='none');
  document.querySelectorAll('.card').forEach(c=>c.classList.remove('open'));
  if(!isOpen){body.style.display='block';card.classList.add('open');}
}

// ── Boxplot ──
function renderBoxChips(){
  const el=document.getElementById('box-chips');
  boxSelected=new Set(DATA.numericCols.slice(0,4));
  el.innerHTML=DATA.numericCols.map(col=>`
    <button class="chip ${boxSelected.has(col)?'at':''}" id="bc${CSS.escape(col)}"
      onclick="toggleBox(this,'${col.replace(/\\/g,'\\\\').replace(/'/g,"\\'")}')">
      ${col.length>18?col.slice(0,18)+'…':col}
    </button>`).join('');
}
function toggleBox(btn,col){
  if(boxSelected.has(col))boxSelected.delete(col);else boxSelected.add(col);
  btn.classList.toggle('at',boxSelected.has(col));
  renderBoxPlots();
}

function makeBoxSVG(col){
  const s=STATS[col];if(!s)return'';
  const W=320,H=120,PL=22,PR=22,PT=20,PB=32;
  const mn=s.min,mx=s.max,rng=mx-mn||1;
  const sc=v=>PL+((v-mn)/rng)*(W-PL-PR);
  const color='#0ea5e9';
  const bY=PT,bH=H-PT-PB,bY2=PT+bH*.25,bH2=bH*.5;
  let svg=`<svg width="100%" viewBox="0 0 ${W} ${H}" style="display:block;background:#07090d;border-radius:8px;margin-bottom:4px;">`;
  const loX=sc(Math.max(mn,s.lo)),hiX=sc(Math.min(mx,s.hi));
  svg+=`<line x1="${loX.toFixed(1)}" y1="${bY+bH/2}" x2="${sc(s.q1).toFixed(1)}" y2="${bY+bH/2}" stroke="${color}" stroke-width="1.5" opacity=".6"/>`;
  svg+=`<line x1="${sc(s.q3).toFixed(1)}" y1="${bY+bH/2}" x2="${hiX.toFixed(1)}" y2="${bY+bH/2}" stroke="${color}" stroke-width="1.5" opacity=".6"/>`;
  svg+=`<line x1="${loX.toFixed(1)}" y1="${bY2}" x2="${loX.toFixed(1)}" y2="${bY2+bH2}" stroke="${color}" stroke-width="1.5" opacity=".6"/>`;
  svg+=`<line x1="${hiX.toFixed(1)}" y1="${bY2}" x2="${hiX.toFixed(1)}" y2="${bY2+bH2}" stroke="${color}" stroke-width="1.5" opacity=".6"/>`;
  svg+=`<rect x="${sc(s.q1).toFixed(1)}" y="${bY2}" width="${(sc(s.q3)-sc(s.q1)).toFixed(1)}" height="${bH2}" fill="${color}22" stroke="${color}" stroke-width="1.5" rx="2"/>`;
  svg+=`<line x1="${sc(s.med).toFixed(1)}" y1="${bY2}" x2="${sc(s.med).toFixed(1)}" y2="${bY2+bH2}" stroke="#00c9a7" stroke-width="2.5"/>`;
  svg+=`<circle cx="${sc(s.mean).toFixed(1)}" cy="${bY+bH/2}" r="3" fill="#f59e0b"/>`;
  s.outliers.forEach(v=>{svg+=`<circle cx="${sc(v).toFixed(1)}" cy="${bY+bH/2}" r="3" fill="#ef4444" opacity=".8"/>`;});
  svg+=`<text x="${sc(mn).toFixed(1)}" y="${H-6}" text-anchor="middle" font-size="8" fill="#4a6180">${mn.toFixed(2)}</text>`;
  svg+=`<text x="${sc(s.med).toFixed(1)}" y="${H-6}" text-anchor="middle" font-size="8" fill="#00c9a7">${s.med.toFixed(2)}</text>`;
  svg+=`<text x="${sc(mx).toFixed(1)}" y="${H-6}" text-anchor="middle" font-size="8" fill="#4a6180">${mx.toFixed(2)}</text>`;
  svg+=`<circle cx="12" cy="12" r="3" fill="#00c9a7"/><text x="18" y="15" font-size="8" fill="#4a6180">Mediana</text>`;
  svg+=`<circle cx="75" cy="12" r="3" fill="#f59e0b"/><text x="81" y="15" font-size="8" fill="#4a6180">Média</text>`;
  if(s.outliers.length)svg+=`<circle cx="125" cy="12" r="3" fill="#ef4444"/><text x="131" y="15" font-size="8" fill="#4a6180">Outlier</text>`;
  svg+=`</svg>`;return svg;
}

function renderBoxPlots(){
  const el=document.getElementById('box-charts');
  if(!boxSelected.size){el.innerHTML='<div style="color:#4a6180;text-align:center;padding:20px;">Selecione variáveis acima.</div>';return;}
  el.innerHTML=[...boxSelected].map(col=>`
    <div class="card" style="margin-bottom:10px;padding:12px 14px;">
      <div style="font-size:11px;font-weight:700;font-family:monospace;color:#0ea5e9;margin-bottom:6px;">${col}</div>
      ${makeBoxSVG(col)}
      <div style="display:flex;gap:10px;margin-top:6px;font-size:10px;color:#4a6180;justify-content:center;flex-wrap:wrap;">
        <span>IQR: ${STATS[col].iqr.toFixed(3)}</span>
        <span>Outliers: ${STATS[col].outliers.length}</span>
        <span>CV: ${(STATS[col].std/STATS[col].mean*100).toFixed(1)}%</span>
      </div>
    </div>`).join('');
}

// ── IA ──
async function handleAI(){
  if(!DATA||CORRS.length===0)return;
  document.getElementById('ai-form').style.display='none';
  document.getElementById('ai-loading').style.display='block';
  document.getElementById('ai-out').style.display='none';
  const ctx=document.getElementById('ai-ctx').value;
  const top10=CORRS.slice(0,10);
  const strong=CORRS.filter(p=>Math.abs(p.r)>=0.7);
  const statsSummary=Object.entries(STATS).map(([k,v])=>
    `${k}: média=${v.mean.toFixed(3)}, std=${v.std.toFixed(3)}, min=${v.min.toFixed(3)}, max=${v.max.toFixed(3)}, outliers=${v.outliers.length}`
  ).join('\n');
  const baseInfo=baseMeta?`\nTAG BASE: ${baseMeta.tag} (${baseMeta.label})`:'';
  const system=`Você é engenheiro especialista em recuperação química de celulose kraft — caldeira de recuperação, evaporação de licor preto, forno de cal, caustificação. Analisa dados do PI Vision da Klabin Otacílio Costa. Seja direto, técnico, prático. Português. Explique mecanismos físico-químicos das correlações.`;
  const user=`Analise e forneça diagnóstico técnico.
${baseInfo}
CONTEXTO: ${ctx||'Análise geral do processo'}
VARIÁVEIS (${DATA.numericCols.length}): ${DATA.numericCols.join(', ')}
ESTATÍSTICAS:\n${statsSummary}
TOP 10 CORRELAÇÕES:\n${top10.map((p,i)=>`${i+1}. ${p.h1} × ${p.h2}: r=${p.r.toFixed(3)} (n=${p.n})`).join('\n')}
CORRELAÇÕES FORTES (|r|≥0.7):\n${strong.length?strong.map(p=>`• ${p.h1} × ${p.h2}: r=${p.r.toFixed(3)}`).join('\n'):'Nenhuma'}

Forneça:
1. DIAGNÓSTICO PRINCIPAL — estado atual do processo
2. CORRELAÇÕES CRÍTICAS — mecanismo físico-químico das 3 mais relevantes
3. ANOMALIAS — variáveis com desvio alto, outliers ou comportamento inesperado
4. AÇÕES RECOMENDADAS — 3 a 5 ações práticas e imediatas
5. O QUE INVESTIGAR — tags adicionais que completariam o diagnóstico`;

  try{
    document.getElementById('ai-prog').textContent='Processando '+CORRS.length+' pares de correlação...';
    const resp=await fetch('https://api.anthropic.com/v1/messages',{
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body:JSON.stringify({model:'claude-sonnet-4-6',max_tokens:1500,system,messages:[{role:'user',content:user}]})
    });
    const d=await resp.json();
    document.getElementById('ai-text').textContent=d.content?.[0]?.text||'Sem resposta.';
    document.getElementById('ai-loading').style.display='none';
    document.getElementById('ai-out').style.display='block';
  }catch(e){
    document.getElementById('ai-text').textContent='Erro: '+e.message;
    document.getElementById('ai-loading').style.display='none';
    document.getElementById('ai-out').style.display='block';
  }
}
function resetAI(){
  document.getElementById('ai-form').style.display='block';
  document.getElementById('ai-out').style.display='none';
  document.getElementById('ai-loading').style.display='none';
}
</script>
</body>
</html>
