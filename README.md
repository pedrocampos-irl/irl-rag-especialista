<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>IRL RAG Specialist Desktop</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;600;700&family=DM+Sans:wght@300;400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/mammoth/1.6.0/mammoth.browser.min.js"></script>
<style>
:root{--bg:#07101b;--surface:#0d1728;--surface2:#13233e;--gold:#d2a23c;--teal:#22c7b8;--green:#57d98c;--purple:#c596ff;--danger:#f07171;--text:#e7e0d2;--dim:rgba(231,224,210,.55);--border:rgba(210,162,60,.2);--mono:'JetBrains Mono',monospace}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;overflow:hidden;background:radial-gradient(circle at top,#13233e,#07101b 55%)}
body{font-family:'DM Sans',sans-serif;color:var(--text);display:flex;flex-direction:column}
button,input,textarea,select{font:inherit}
.header{display:flex;align-items:center;gap:12px;padding:12px 16px;background:#0b1220;border-bottom:1px solid var(--border)}
.logo{width:36px;height:36px;border-radius:8px;background:linear-gradient(135deg,#efc86b,#b67e17);display:flex;align-items:center;justify-content:center;color:#07101b;font:600 11px var(--mono)}
.title h1{font:700 22px 'Cormorant Garamond',serif}
.title p,.small{font-size:11px;color:var(--dim);line-height:1.5}
.pill{padding:3px 8px;border-radius:99px;border:1px solid;font:500 9px var(--mono)}
.rag{color:var(--teal);border-color:rgba(34,199,184,.3)}
.desk{color:var(--gold);border-color:rgba(210,162,60,.3)}
.spacer{margin-left:auto}
.main{display:flex;flex:1;overflow:hidden}
.side{width:350px;background:rgba(8,13,22,.95);border-right:1px solid var(--border);display:flex;flex-direction:column}
.tabs{display:flex;border-bottom:1px solid var(--border)}
.tab{flex:1;padding:10px 6px;text-align:center;cursor:pointer;color:var(--dim);font:500 10px var(--mono)}
.tab.active{color:var(--gold);background:rgba(210,162,60,.06)}
.panel{display:none;flex:1;overflow:hidden;flex-direction:column}
.panel.active{display:flex}
.section{padding:12px 14px;border-bottom:1px solid var(--border)}
.label,.listTitle{font:500 9px var(--mono);letter-spacing:.12em;color:rgba(210,162,60,.8);text-transform:uppercase}
.label{margin-bottom:8px}
.field{display:flex;flex-direction:column;gap:5px;margin-bottom:8px}
.field:last-child{margin-bottom:0}
.field label{font-size:11px;color:rgba(231,224,210,.75)}
.input,.select,.textarea{width:100%;background:var(--surface2);border:1px solid var(--border);border-radius:8px;color:var(--text);padding:8px 10px;outline:none}
.textarea{resize:vertical;min-height:70px}
.row{display:flex;gap:8px}
.row>*{flex:1}
.btn{padding:9px 10px;border-radius:8px;border:1px solid var(--border);background:none;color:var(--text);cursor:pointer}
.btn.primary{background:linear-gradient(135deg,#d8ae4a,#b78220);border:none;color:#07101b;font-weight:600}
.btn.secondary{color:var(--teal);border-color:rgba(34,199,184,.28)}
.btn.warn{color:var(--danger);border-color:rgba(240,113,113,.25)}
.upload{position:relative;border:1px dashed rgba(210,162,60,.45);border-radius:10px;padding:14px;text-align:center;background:rgba(210,162,60,.04);cursor:pointer}
.upload.drag{background:rgba(210,162,60,.09)}
.upload input{position:absolute;inset:0;opacity:0;cursor:pointer}
.progress{height:4px;background:rgba(255,255,255,.08);border-radius:99px;overflow:hidden;margin-top:8px;display:none}
.progress.on{display:block}
.progress span{display:block;height:100%;width:0;background:linear-gradient(90deg,#d8ae4a,#22c7b8)}
.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:6px}
.stat{background:var(--surface2);border-radius:8px;padding:8px 6px;text-align:center}
.stat strong{display:block;font:500 15px var(--mono);color:var(--gold)}
.stat span{font-size:9px;color:var(--dim)}
.listTitle{padding:8px 14px 0}
.list{flex:1;overflow:auto;padding:10px 12px}
.list::-webkit-scrollbar,.messages::-webkit-scrollbar{width:4px}
.list::-webkit-scrollbar-thumb,.messages::-webkit-scrollbar-thumb{background:rgba(210,162,60,.25);border-radius:99px}
.empty{padding:24px 12px;text-align:center;color:var(--dim);font-size:12px;line-height:1.6}
.card{background:var(--surface2);border-radius:10px;padding:10px 11px;margin-bottom:8px}
.head{display:flex;align-items:center;gap:8px}
.growText{flex:1;min-width:0;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;font-size:12px;font-weight:500}
.meta{margin-top:4px;font:400 10px var(--mono);color:var(--dim);line-height:1.5}
.badge{padding:1px 6px;border-radius:99px;font:500 8px var(--mono);border:1px solid}
.local{color:var(--gold);border-color:rgba(210,162,60,.25)}
.ok{color:var(--green);border-color:rgba(87,217,140,.25)}
.fix{color:var(--purple);border-color:rgba(197,150,255,.25)}
.iconBtn{border:none;background:none;color:var(--dim);cursor:pointer;font-size:13px}
.chat{display:flex;flex:1;flex-direction:column;overflow:hidden}
.pipe{display:flex;align-items:center;gap:5px;padding:8px 14px;background:#0b1220;border-bottom:1px solid var(--border);font:500 9px var(--mono);color:var(--dim)}
.step{display:flex;align-items:center;gap:4px;padding:3px 8px;border:1px solid transparent;border-radius:99px}
.step.active{color:var(--gold);border-color:rgba(210,162,60,.35)}
.step.done{color:var(--teal);border-color:rgba(34,199,184,.28)}
.dot{width:5px;height:5px;border-radius:50%;background:currentColor}
.retrieve{margin-left:auto;color:var(--teal)}
.messages{flex:1;overflow:auto;padding:20px;display:flex;flex-direction:column;gap:16px}
.welcome{margin:auto;text-align:center;max-width:700px;color:rgba(231,224,210,.78)}
.welcome h2{font:700 30px 'Cormorant Garamond',serif;color:#fff5df;margin:10px 0 8px}
.welcome p{font-size:14px;line-height:1.7}
.hints{display:grid;grid-template-columns:repeat(2,1fr);gap:8px;margin-top:16px}
.hint{padding:10px 12px;border:1px solid var(--border);border-radius:10px;background:rgba(255,255,255,.03);font-size:12px;text-align:left;cursor:pointer}
.msg{display:flex;gap:10px;max-width:980px}
.msg.user{align-self:flex-end;flex-direction:row-reverse}
.av{width:30px;height:30px;border-radius:8px;display:flex;align-items:center;justify-content:center;flex-shrink:0}
.msg.user .av{background:rgba(210,162,60,.12);border:1px solid rgba(210,162,60,.25)}
.msg.assistant .av{background:rgba(34,199,184,.12);border:1px solid rgba(34,199,184,.25)}
.content{flex:1;min-width:0}
.bubble{background:var(--surface2);border:1px solid var(--border);border-radius:12px;padding:13px 14px;font-size:14px;line-height:1.7}
.msg.user .bubble{background:rgba(210,162,60,.07)}
.bubble h3{font:700 18px 'Cormorant Garamond',serif;color:#f3c766;margin:10px 0 4px}
.bubble p{margin:0 0 8px}
.bubble ul{padding-left:18px;margin:0 0 8px}
.bubble code{font:12px var(--mono);background:rgba(255,255,255,.06);padding:1px 4px;border-radius:4px}
.scopes{display:flex;flex-wrap:wrap;gap:5px;margin-top:8px}
.scope{padding:3px 8px;border-radius:99px;font:500 9px var(--mono);border:1px solid}
.ma{color:var(--green);border-color:rgba(87,217,140,.24)}
.sst{color:#ffb562;border-color:rgba(255,181,98,.24)}
.rs{color:var(--purple);border-color:rgba(197,150,255,.24)}
.trab{color:#67a6ff;border-color:rgba(103,166,255,.24)}
.esg{color:var(--teal);border-color:rgba(34,199,184,.24)}
.ql{color:#f3c766;border-color:rgba(243,199,102,.24)}
.sa{color:#8cd7ff;border-color:rgba(140,215,255,.24)}
.si{color:var(--danger);border-color:rgba(240,113,113,.24)}
.sources{margin-top:8px;border:1px solid var(--border);border-radius:10px;overflow:hidden}
.srcHead{padding:7px 10px;background:rgba(34,199,184,.06);font:500 9px var(--mono);color:var(--teal);cursor:pointer}
.srcBody{display:none;padding:8px 10px}
.srcBody.open{display:block}
.src{padding:8px 9px;border-left:2px solid var(--gold);background:#0f1b30;border-radius:6px;margin-bottom:7px}
.src.rep{border-left-color:var(--green)}
.src.correction{border-left-color:var(--purple)}
.src.hist{border-left-color:#67a6ff}
.src strong{display:block;font:500 9px var(--mono);margin-bottom:4px}
.src p{font-size:12px;color:rgba(231,224,210,.78)}
.feedback{display:flex;align-items:center;gap:8px;margin-top:8px;padding:8px 10px;border:1px solid var(--border);border-radius:10px}
.feedback span{flex:1;font:500 9px var(--mono);color:var(--dim)}
.feedback button{padding:5px 10px;border-radius:8px;border:1px solid var(--border);background:none;color:var(--text);cursor:pointer}
.feedback .okBtn.active{color:var(--green);border-color:rgba(87,217,140,.28)}
.feedback .badBtn.active{color:var(--purple);border-color:rgba(197,150,255,.28)}
.corr{margin-top:8px;padding:10px;border:1px solid rgba(197,150,255,.26);border-radius:10px;background:rgba(197,150,255,.05)}
.corr label{display:block;margin-bottom:6px;font:500 9px var(--mono);color:var(--purple)}
.typing{display:flex;gap:5px}
.typing i{width:7px;height:7px;border-radius:50%;background:var(--teal);animation:p 1.2s infinite}
.typing i:nth-child(2){animation-delay:.2s}
.typing i:nth-child(3){animation-delay:.4s}
@keyframes p{0%,100%{opacity:.25;transform:scale(.75)}50%{opacity:1;transform:scale(1)}}
.composer{padding:12px 14px;border-top:1px solid var(--border);background:#0b1220}
.composerRow{display:flex;gap:10px;align-items:flex-end}
.composer textarea{flex:1;min-height:46px;max-height:140px;resize:none}
.composer .meta{display:flex;justify-content:space-between;margin-top:6px}
.toast{position:fixed;left:50%;bottom:16px;transform:translateX(-50%) translateY(12px);opacity:0;pointer-events:none;padding:9px 14px;border-radius:10px;background:#0b1220;border:1px solid rgba(210,162,60,.35);transition:.22s}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
</style>
</head>
<body>
<div class="header"><div class="logo">IRL</div><div class="title"><h1>IRL RAG Specialist</h1><p>App desktop local com base persistente no navegador</p></div><span class="pill rag">RAG + BM25</span><span class="pill desk">DESKTOP</span><div class="spacer small">Os dados ficam neste computador.</div></div>
<div class="main">
<aside class="side">
<div class="tabs"><div class="tab active" data-tab="base">BASE</div><div class="tab" data-tab="config">CONFIG</div><div class="tab" data-tab="memory">MEMORIA</div></div>
<section class="panel active" id="panel-base">
<div class="section"><div class="label">Upload Local</div><label class="upload" id="uploadZone"><input type="file" id="fileInput" multiple accept=".txt,.pdf,.doc,.docx,.rtf"><strong>Arraste arquivos ou clique para selecionar</strong><div class="small">PDF, TXT, DOC, DOCX e RTF</div><div class="progress" id="progress"><span id="progressFill"></span></div></label></div>
<div class="section"><div class="label">Modo Desktop</div><div class="small">Esta versao usa upload local. A integracao com Google Drive foi removida desta edicao porque autenticacao OAuth em arquivo local nao e confiavel.</div></div>
<div class="section"><button class="btn secondary" id="analyzeDocsBtn" disabled>Gerar analise dos documentos</button><div style="height:10px"></div><div class="stats"><div class="stat"><strong id="statDocs">0</strong><span>Docs</span></div><div class="stat"><strong id="statChunks">0</strong><span>Chunks</span></div><div class="stat"><strong id="statRep">0</strong><span>Repertorio</span></div><div class="stat"><strong id="statHist">0</strong><span>Historico</span></div></div><div style="height:10px"></div><div class="row"><div class="field"><label>Top-K</label><input class="input" id="cfgTopK" type="number" min="1" max="12" value="5"></div><div class="field"><label>Chunk</label><input class="input" id="cfgChunk" type="number" min="300" max="1800" step="100" value="900"></div></div><div class="small" style="margin-top:8px">Ao mudar o chunk, a base salva sera reprocessada automaticamente.</div></div>
<div class="listTitle">Documentos Indexados</div><div class="list" id="docList"></div>
</section>
<section class="panel" id="panel-config">
<div class="section"><div class="label">IA</div><div class="field"><label>Provider</label><select class="select" id="provider"><option value="gemini">Google Gemini</option></select></div><div class="field"><label>Chave da API</label><input class="input" id="apiKey" type="password" placeholder="Cole sua chave aqui"></div><div class="field"><label>Modelo</label><select class="select" id="model"><option value="gemini-2.0-flash">gemini-2.0-flash</option><option value="gemini-1.5-pro">gemini-1.5-pro</option><option value="gemini-2.5-pro-exp-03-25">gemini-2.5-pro-exp-03-25</option></select></div><div class="row"><button class="btn primary" id="saveConfigBtn">Salvar</button><button class="btn" id="testApiBtn">Testar API</button></div><div class="small">A chave fica salva apenas neste navegador.</div></div>
<div class="section"><div class="label">Prompt do Especialista</div><textarea class="textarea" id="systemPrompt"></textarea></div>
<div class="section"><div class="label">Exportacao e Limpeza</div><div class="row"><button class="btn" id="exportBackupBtn">Exportar Backup</button><button class="btn warn" id="clearKbBtn">Limpar Base</button></div><div style="height:8px"></div><div class="row"><button class="btn" id="exportHistoryBtn">Exportar Historico</button><button class="btn warn" id="clearHistoryBtn">Limpar Historico</button></div><div style="height:8px"></div><div class="row"><button class="btn" id="exportRepBtn">Exportar Repertorio</button><button class="btn warn" id="clearRepBtn">Limpar Repertorio</button></div></div>
</section>
<section class="panel" id="panel-memory"><div class="listTitle">Repertorio</div><div class="list" id="repList"></div><div class="listTitle">Historico</div><div class="list" id="histList"></div></section>
</aside>
<main class="chat">
<div class="pipe"><div class="step" id="step-query"><span class="dot"></span>QUERY</div><div class="step" id="step-retrieve"><span class="dot"></span>RETRIEVE</div><div class="step" id="step-augment"><span class="dot"></span>AUGMENT</div><div class="step" id="step-generate"><span class="dot"></span>GENERATE</div><div class="step" id="step-feedback"><span class="dot"></span>FEEDBACK</div><div class="retrieve" id="retrieveInfo"></div></div>
<div class="messages" id="messages"><div class="welcome" id="welcome"><div style="font-size:46px;opacity:.3">⚖</div><h2>Versao funcional para desktop</h2><p>Envie documentos locais, configure a chave da API e faca perguntas com memoria local, repertorio validado e recuperacao RAG.</p><div class="hints"><div class="hint" data-hint="Esta norma e aplicavel ao escopo de Meio Ambiente? Analise detalhadamente e justifique.">Aplicabilidade ao escopo de MA</div><div class="hint" data-hint="Esta norma deve ser cadastrada no CAL ou encaminhada ao CAL de Exclusoes? Justifique.">CAL ou CAL de Exclusoes</div><div class="hint" data-hint="Analise a aplicabilidade nos escopos MA, SST e RS com justificativa tecnica.">Analise MA, SST e RS</div><div class="hint" data-hint="Existe norma similar ou conflitante no repertorio validado pela equipe?">Verificar repertorio validado</div></div></div></div>
<div class="composer"><div class="composerRow"><textarea class="textarea" id="userInput" rows="1" placeholder="Descreva a norma, cole o texto ou faca sua pergunta..."></textarea><button class="btn primary" id="sendBtn" style="width:48px;height:48px;padding:0">➤</button></div><div class="meta"><span>Enter envia. Shift+Enter quebra linha.</span><button class="btn" id="clearChatBtn" style="padding:4px 10px">Limpar chat</button></div></div>
</main>
</div>
<div class="toast" id="toast"></div>
<script>
const DEFAULT_PROMPT=`Voce e um Especialista Senior em Gestao de Requisitos Legais da Ius Natura, atuando no setor IRL.

Diretrizes:
1. Respostas tecnicas, estruturadas e justificadas.
2. Se faltarem dados, peca esclarecimentos antes de concluir.
3. Nao invente fatos.
4. Ao usar trechos recuperados, cite a fonte.

Estrutura obrigatoria:
### Classificacao da Aplicabilidade
### Escopo(s) Envolvido(s)
### Justificativa Tecnica
### Orientacao de Analise no Sistema CAL
### Pontos de Atencao

Prioridade de contexto:
- [REPERTORIO VALIDADO]
- [CORRECAO DA EQUIPE]
- [BASE DOCUMENTAL]
- [HISTORICO]

Responda sempre em portugues do Brasil.`;

const STOP=new Set(['de','a','o','que','e','do','da','em','um','para','com','uma','os','no','se','na','por','mais','as','dos','como','mas','ao','ele','das','seu','sua','ou','ser','quando','muito','ha','nos','nas','este','esta','isso','essa','esse','entre','sobre','pelas','pelos','pela','pelo','sao','foi','sera','tambem','sem']);
const DB_NAME='irl-rag-desktop-db';
const DB_VERSION=1;
const STORE_DOCS='docs';
const STORE_KV='kv';
const MAX_CHAT_TURNS=6;
const MAX_CONTEXT_CHARS=9000;
const state={docs:[],chunks:[],repertoire:[],history:[],chatHistory:[],idf:{},busy:false,msgCounter:0,db:null};
const el=id=>document.getElementById(id);
const ui={docList:el('docList'),repList:el('repList'),histList:el('histList'),messages:el('messages'),userInput:el('userInput'),sendBtn:el('sendBtn'),toast:el('toast'),progress:el('progress'),progressFill:el('progressFill'),retrieveInfo:el('retrieveInfo')};

if(typeof pdfjsLib!=='undefined') pdfjsLib.GlobalWorkerOptions.workerSrc='https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

const uid=()=>Date.now().toString(36)+Math.random().toString(36).slice(2,8);
const wait=ms=>new Promise(r=>setTimeout(r,ms));
const esc=s=>String(s??'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
const fmt=iso=>{if(!iso)return'';const d=new Date(iso);return d.toLocaleDateString('pt-BR')+' '+d.toLocaleTimeString('pt-BR',{hour:'2-digit',minute:'2-digit'});};
const fdate=()=>new Date().toISOString().slice(0,10);

function showToast(msg){
  ui.toast.textContent=msg;
  ui.toast.className='toast show';
  clearTimeout(showToast.t);
  showToast.t=setTimeout(()=>ui.toast.className='toast',3200);
}

function setProgress(on,pct=0){
  ui.progress.classList.toggle('on',on);
  ui.progressFill.style.width=pct+'%';
}

function saveSettings(){
  localStorage.setItem('irl-settings',JSON.stringify({
    apiKey:el('apiKey').value.trim(),
    model:el('model').value,
    provider:el('provider').value,
    prompt:el('systemPrompt').value,
    topK:Number(el('cfgTopK').value)||5,
    chunk:Number(el('cfgChunk').value)||900
  }));
}

function loadSettings(){
  const s=JSON.parse(localStorage.getItem('irl-settings')||'{}');
  el('apiKey').value=s.apiKey||'';
  el('model').value=s.model||'gemini-2.0-flash';
  el('provider').value=s.provider||'gemini';
  el('systemPrompt').value=s.prompt||DEFAULT_PROMPT;
  el('cfgTopK').value=s.topK||5;
  el('cfgChunk').value=s.chunk||900;
}

function openDb(){
  return new Promise((resolve,reject)=>{
    const req=indexedDB.open(DB_NAME,DB_VERSION);
    req.onerror=()=>reject(req.error);
    req.onupgradeneeded=()=>{
      const db=req.result;
      if(!db.objectStoreNames.contains(STORE_DOCS)) db.createObjectStore(STORE_DOCS,{keyPath:'id'});
      if(!db.objectStoreNames.contains(STORE_KV)) db.createObjectStore(STORE_KV,{keyPath:'key'});
    };
    req.onsuccess=()=>resolve(req.result);
  });
}

function dbPut(store,val){
  return new Promise((resolve,reject)=>{
    const tx=state.db.transaction(store,'readwrite');
    tx.objectStore(store).put(val);
    tx.oncomplete=()=>resolve(true);
    tx.onerror=()=>reject(tx.error);
  });
}

function dbGet(store,key){
  return new Promise((resolve,reject)=>{
    const tx=state.db.transaction(store,'readonly');
    const req=tx.objectStore(store).get(key);
    req.onsuccess=()=>resolve(req.result||null);
    req.onerror=()=>reject(req.error);
  });
}

function dbGetAll(store){
  return new Promise((resolve,reject)=>{
    const tx=state.db.transaction(store,'readonly');
    const req=tx.objectStore(store).getAll();
    req.onsuccess=()=>resolve(req.result||[]);
    req.onerror=()=>reject(req.error);
  });
}

function dbDelete(store,key){
  return new Promise((resolve,reject)=>{
    const tx=state.db.transaction(store,'readwrite');
    tx.objectStore(store).delete(key);
    tx.oncomplete=()=>resolve(true);
    tx.onerror=()=>reject(tx.error);
  });
}

async function loadDbData(){
  state.db=await openDb();
  state.docs=await dbGetAll(STORE_DOCS);
  state.repertoire=(await dbGet(STORE_KV,'repertoire'))?.value||[];
  state.history=(await dbGet(STORE_KV,'history'))?.value||[];
  rebuildIndexes();
}

async function saveMemory(){
  await dbPut(STORE_KV,{key:'repertoire',value:state.repertoire});
  await dbPut(STORE_KV,{key:'history',value:state.history});
}

function tokenize(t){
  return String(t||'').toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'').replace(/[^\w\s]/g,' ').split(/\s+/).filter(x=>x.length>2&&!STOP.has(x));
}

function buildChunks(text,name,id,source){
  const size=Number(el('cfgChunk').value)||900;
  const overlap=Math.floor(size*.15);
  const chunks=[];let start=0;let n=0;
  while(start<text.length){
    const end=Math.min(start+size,text.length);
    const piece=text.slice(start,end).trim();
    if(piece.length>80) chunks.push({id:`${id}_${n++}`,text:piece,docName:name,docId:id,source,type:'doc'});
    start+=Math.max(120,size-overlap);
  }
  return chunks;
}

function retrievalChunks(){
  const reps=state.repertoire.map(r=>({id:'rep_'+r.id,text:`Pergunta: ${r.query}\nResposta: ${r.answer}${r.correction?'\nCorrecao: '+r.correction:''}`,docName:r.type==='approved'?'Repertorio Validado':'Correcao da Equipe',type:r.type==='approved'?'rep':'correction'}));
  const hist=state.history.slice(-40).map(h=>({id:'hist_'+h.id,text:`Pergunta: ${h.query}\nResposta: ${h.answer}${h.correction?'\nCorrecao: '+h.correction:''}`,docName:'Historico',type:'hist'}));
  return [...state.chunks,...reps,...hist];
}

function rebuildIndexes(){
  state.chunks=state.docs.flatMap(d=>d.chunks||[]);
  const all=retrievalChunks(),df={},N=Math.max(all.length,1);
  all.forEach(c=>[...new Set(tokenize(c.text))].forEach(t=>df[t]=(df[t]||0)+1));
  state.idf={};
  Object.entries(df).forEach(([t,f])=>state.idf[t]=Math.log((N+1)/(f+1))+1);
}

function bm25(query,chunk,avgdl){
  const qt=tokenize(query),ct=tokenize(chunk.text),dl=ct.length;
  if(!dl) return 0;
  const tf={};ct.forEach(t=>tf[t]=(tf[t]||0)+1);
  const k1=1.5,b=.75;let score=0;
  qt.forEach(q=>{const f=tf[q]||0;if(!f)return;score+=(state.idf[q]||1)*(f*(k1+1))/(f+k1*(1-b+b*dl/avgdl));});
  if(chunk.type==='rep') score*=1.4;
  if(chunk.type==='correction') score*=1.6;
  return score;
}

function retrieve(query,k){
  const all=retrievalChunks();if(!all.length)return[];
  const avg=all.reduce((s,c)=>s+Math.max(1,tokenize(c.text).length),0)/all.length;
  return all.map(chunk=>({chunk,score:bm25(query,chunk,avg)})).filter(x=>x.score>0).sort((a,b)=>b.score-a.score).slice(0,k);
}

async function extractText(file){
  const ext=file.name.split('.').pop().toLowerCase();
  if(ext==='txt'||ext==='rtf') return await file.text();
  if(ext==='pdf'){
    const buf=await file.arrayBuffer();
    const pdf=await pdfjsLib.getDocument({data:buf}).promise;
    let text='';
    for(let i=1;i<=pdf.numPages;i++){
      const page=await pdf.getPage(i);
      const content=await page.getTextContent();
      text+=content.items.map(x=>x.str).join(' ')+'\n';
    }
    return text;
  }
  if(ext==='doc'||ext==='docx'){
    const buf=await file.arrayBuffer();
    return (await mammoth.extractRawText({arrayBuffer:buf})).value;
  }
  throw new Error('Formato nao suportado: '+ext);
}

async function addDoc({name,size,text,source='local'}){
  const raw=String(text||'').trim();
  if(!raw) throw new Error('Nenhum texto extraido do arquivo.');
  const id=uid(),chunks=buildChunks(raw,name,id,source),doc={id,name,size,source,text:raw,chunks,addedAt:new Date().toISOString()};
  state.docs.push(doc);
  await dbPut(STORE_DOCS,doc);
  rebuildIndexes();
  renderAll();
  el('analyzeDocsBtn').disabled=state.docs.length===0;
}

async function reprocessAllDocs(){
  for(const doc of state.docs){
    doc.chunks=buildChunks(doc.text,doc.name,doc.id,doc.source);
    await dbPut(STORE_DOCS,doc);
  }
  rebuildIndexes();
  renderAll();
}

async function handleFiles(files){
  for(const file of files){
    try{
      setProgress(true,15);
      const text=await extractText(file);
      setProgress(true,75);
      await addDoc({name:file.name,size:file.size,text,source:'local'});
      setProgress(true,100);
      showToast(`Documento "${file.name}" indexado.`);
    }catch(err){
      showToast(`Erro ao processar "${file.name}": ${err.message}`);
    }finally{
      setTimeout(()=>setProgress(false,0),280);
    }
  }
}

function renderDocs(){
  if(!state.docs.length){ui.docList.innerHTML='<div class="empty">Nenhum documento indexado ainda.</div>';return;}
  ui.docList.innerHTML=state.docs.slice().reverse().map(d=>`<div class="card"><div class="head"><div>📄</div><div class="growText" title="${esc(d.name)}">${esc(d.name)}</div><span class="badge local">Local</span><button class="iconBtn" data-del-doc="${d.id}">✕</button></div><div class="meta">${d.chunks.length} chunks · ${(d.size/1024).toFixed(1)} KB · ${fmt(d.addedAt)}</div></div>`).join('');
}

function renderRep(){
  if(!state.repertoire.length){ui.repList.innerHTML='<div class="empty">Nenhum item no repertorio.</div>';return;}
  ui.repList.innerHTML=state.repertoire.slice().reverse().map(r=>`<div class="card"><div class="head"><span class="badge ${r.type==='approved'?'ok':'fix'}">${r.type==='approved'?'VALIDADO':'CORRECAO'}</span><div class="growText">${esc(r.query)}</div></div><div class="meta">${fmt(r.timestamp)}${r.correction?'<br>'+esc(r.correction.slice(0,180)):''}</div></div>`).join('');
}

function renderHist(){
  if(!state.history.length){ui.histList.innerHTML='<div class="empty">Nenhuma interacao registrada.</div>';return;}
  ui.histList.innerHTML=state.history.slice().reverse().map(h=>`<div class="card" data-replay="${h.id}"><div class="head"><div>${h.feedback==='approved'?'✅':h.feedback==='corrected'?'⚠':'💬'}</div><div class="growText">${esc(h.query)}</div></div><div class="meta">${fmt(h.timestamp)}<br>${esc(h.answer.slice(0,180))}${h.answer.length>180?'...':''}</div></div>`).join('');
}

function renderStats(){
  el('statDocs').textContent=state.docs.length;
  el('statChunks').textContent=state.chunks.length;
  el('statRep').textContent=state.repertoire.length;
  el('statHist').textContent=state.history.length;
}

function renderAll(){
  renderStats();
  renderDocs();
  renderRep();
  renderHist();
  bindDynamic();
}

function setPipe(step){
  const order=['query','retrieve','augment','generate','feedback'];
  order.forEach(name=>{
    const e=el('step-'+name);
    e.classList.remove('active','done');
    if(name===step)e.classList.add('active'); else if(order.indexOf(name)<order.indexOf(step)) e.classList.add('done');
  });
}

function resetPipe(){
  ['query','retrieve','augment','generate','feedback'].forEach(n=>el('step-'+n).classList.remove('active','done'));
  ui.retrieveInfo.textContent='';
}

function detectScopes(text){
  const t=String(text||'').toLowerCase(),out=[];
  if(/\b(meio ambiente|ambiental|licenciamento|residuos|emissoes|conama|ibama)\b/.test(t)) out.push(['MA','ma']);
  if(/\b(sst|sso|epi|nr\s?\d|seguranca do trabalho|acidente|ltcat|pcmso)\b/.test(t)) out.push(['SST','sst']);
  if(/\b(responsabilidade social|sa8000)\b/.test(t)) out.push(['RS','rs']);
  if(/\b(trabalhista|clt|empregado|rescisao|ferias|salario|jornada)\b/.test(t)) out.push(['Trabalhista','trab']);
  if(/\b(esg|governanca|sustentabilidade)\b/.test(t)) out.push(['ESG','esg']);
  if(/\b(qualidade|iso 9001)\b/.test(t)) out.push(['QL','ql']);
  if(/\b(seguranca de alimentos|anvisa|alimento|haccp)\b/.test(t)) out.push(['SA','sa']);
  if(/\b(seguranca da informacao|lgpd|iso 27001|privacidade)\b/.test(t)) out.push(['SI','si']);
  return out;
}

function md(text){
  let html=esc(text).replace(/^### (.*)$/gm,'<h3>$1</h3>').replace(/^## (.*)$/gm,'<h3>$1</h3>').replace(/\*\*(.*?)\*\*/g,'<strong>$1</strong>').replace(/`([^`]+)`/g,'<code>$1</code>');
  html=html.split('\n').map(line=>{if(/^<h3>/.test(line)||!line.trim())return line;if(/^- /.test(line))return '<li>'+line.slice(2)+'</li>';return '<p>'+line+'</p>'}).join('\n');
  return html.replace(/(<li>.*<\/li>\n?)+/g,m=>'<ul>'+m+'</ul>');
}

function appendMsg(role,content,retrieved=[]){
  el('welcome')?.remove();
  const wrap=document.createElement('div');wrap.className='msg '+role;
  const id='msg_'+(++state.msgCounter),scopes=role==='assistant'?detectScopes(content):[];
  const scopesHtml=scopes.length?`<div class="scopes">${scopes.map(([l,c])=>`<span class="scope ${c}">${l}</span>`).join('')}</div>`:'';
  const srcHtml=retrieved.length?`<div class="sources"><div class="srcHead" data-toggle="${id}">FONTES RECUPERADAS (${retrieved.length}) ▾</div><div class="srcBody" id="src-${id}">${retrieved.map((r,i)=>`<div class="src ${r.chunk.type==='rep'?'rep':r.chunk.type==='correction'?'correction':r.chunk.type==='hist'?'hist':''}"><strong>${i+1}. ${esc(r.chunk.docName)} · ${r.score.toFixed(3)}</strong><p>${esc(r.chunk.text.slice(0,240))}${r.chunk.text.length>240?'...':''}</p></div>`).join('')}</div></div>`:'';
  const fbHtml=role==='assistant'?`<div class="feedback" id="fb-${id}"><span>VALIDACAO DA EQUIPE</span><button class="okBtn" data-fb="ok">✅ Correta</button><button class="badBtn" data-fb="bad">❌ Corrigir</button></div>`:'';
  wrap.innerHTML=`<div class="av">${role==='user'?'👤':'⚖'}</div><div class="content"><div class="bubble">${role==='assistant'?md(content):esc(content).replace(/\n/g,'<br>')}</div>${scopesHtml}${srcHtml}${fbHtml}</div>`;
  wrap.id=id;
  ui.messages.appendChild(wrap);
  ui.messages.scrollTop=ui.messages.scrollHeight;
  return id;
}

function addTyping(){
  const wrap=document.createElement('div');
  wrap.className='msg assistant';
  wrap.id='typing';
  wrap.innerHTML='<div class="av">⚖</div><div class="content"><div class="bubble"><div class="typing"><i></i><i></i><i></i></div></div></div>';
  ui.messages.appendChild(wrap);
  ui.messages.scrollTop=ui.messages.scrollHeight;
}

function removeTyping(){el('typing')?.remove();}

function ragContext(retrieved){
  if(!retrieved.length) return '';
  const reps=retrieved.filter(r=>r.chunk.type==='rep'||r.chunk.type==='correction');
  const docs=retrieved.filter(r=>r.chunk.type==='doc');
  const hist=retrieved.filter(r=>r.chunk.type==='hist');
  const compact=list=>list.map((r,i)=>`[${i+1}] ${r.chunk.docName}\n${r.chunk.text.slice(0,1200)}`).join('\n\n');
  let ctx='\n\n---\n## CONTEXTO RECUPERADO\n';
  if(reps.length) ctx+='\n### [REPERTORIO VALIDADO]\n'+compact(reps);
  if(docs.length) ctx+='\n\n### [BASE DOCUMENTAL]\n'+compact(docs);
  if(hist.length) ctx+='\n\n### [HISTORICO]\n'+compact(hist);
  return (ctx+'\n\nUse esse contexto apenas quando pertinente e cite as fontes utilizadas.').slice(0,MAX_CONTEXT_CHARS);
}

function recentChatHistory(){
  return state.chatHistory.slice(-(MAX_CHAT_TURNS*2));
}

async function callGemini(messages){
  const key=el('apiKey').value.trim();
  if(!key) throw new Error('Informe a chave da API na aba CONFIG.');
  const body={
    systemInstruction:{parts:[{text:el('systemPrompt').value.trim()||DEFAULT_PROMPT}]},
    contents:messages.map(m=>({role:m.role==='assistant'?'model':'user',parts:[{text:m.content}]})),
    generationConfig:{temperature:.2,topP:.9,maxOutputTokens:2048}
  };
  const res=await fetch(`https://generativelanguage.googleapis.com/v1beta/models/${encodeURIComponent(el('model').value)}:generateContent?key=${encodeURIComponent(key)}`,{
    method:'POST',
    headers:{'Content-Type':'application/json'},
    body:JSON.stringify(body)
  });
  const data=await res.json();
  if(!res.ok) throw new Error(data?.error?.message||'Falha na API.');
  const text=(data.candidates||[]).flatMap(c=>c.content?.parts||[]).map(p=>p.text||'').join('\n').trim();
  if(!text) throw new Error('A API retornou vazio.');
  return text;
}

async function sendMessage(){
  const query=ui.userInput.value.trim();
  if(!query||state.busy) return;
  state.busy=true;
  ui.sendBtn.disabled=true;
  appendMsg('user',query);
  ui.userInput.value='';
  autoGrow();
  setPipe('query');
  await wait(180);
  setPipe('retrieve');
  const topK=Math.max(1,Number(el('cfgTopK').value)||5);
  const retrieved=retrieve(query,topK);
  const repN=retrieved.filter(r=>r.chunk.type==='rep'||r.chunk.type==='correction').length;
  const histN=retrieved.filter(r=>r.chunk.type==='hist').length;
  ui.retrieveInfo.textContent=retrieved.length?`${retrieved.length} fontes (${retrieved.length-repN-histN} docs · ${repN} rep. · ${histN} hist.)`:'sem recuperacao';
  await wait(160);
  setPipe('augment');
  const prompt=query+ragContext(retrieved);
  await wait(140);
  setPipe('generate');
  addTyping();
  let answer='';
  try{
    answer=await callGemini([...recentChatHistory(),{role:'user',content:prompt}]);
  }catch(err){
    answer=`### Erro\nNao foi possivel gerar a resposta.\n\n**Detalhe:** ${err.message}`;
  }
  removeTyping();
  const msgId=appendMsg('assistant',answer,retrieved);
  wireFeedback(msgId,query,answer);
  state.chatHistory.push({role:'user',content:query},{role:'assistant',content:answer});
  if(state.chatHistory.length>(MAX_CHAT_TURNS*2)) state.chatHistory=state.chatHistory.slice(-(MAX_CHAT_TURNS*2));
  state.history.push({id:uid(),query,answer,feedback:null,correction:null,sources:retrieved.map(r=>r.chunk.docName),timestamp:new Date().toISOString()});
  if(state.history.length>500) state.history=state.history.slice(-500);
  await saveMemory();
  renderAll();
  setPipe('feedback');
  await wait(120);
  resetPipe();
  state.busy=false;
  ui.sendBtn.disabled=false;
  ui.userInput.focus();
}

function wireFeedback(msgId,query,answer){
  const box=el('fb-'+msgId);if(!box)return;
  box.querySelector('[data-fb="ok"]').onclick=()=>submitFeedback(query,answer,'approved',box);
  box.querySelector('[data-fb="bad"]').onclick=()=>openCorrectionForm(query,answer,box);
}

async function submitFeedback(query,answer,type,box,correction=''){
  state.repertoire.push({id:uid(),type,query,answer,correction,timestamp:new Date().toISOString()});
  const hist=[...state.history].reverse().find(x=>x.query===query&&x.answer===answer);
  if(hist){
    hist.feedback=type==='approved'?'approved':'corrected';
    hist.correction=correction||null;
  }
  await saveMemory();
  rebuildIndexes();
  renderAll();
  if(box){
    if(type==='approved'){
      box.querySelector('.okBtn').classList.add('active');
      box.querySelector('.badBtn').style.display='none';
    }
  }
  showToast(type==='approved'?'Resposta validada e salva no repertorio.':'Correcao salva no repertorio.');
}

function openCorrectionForm(query,answer,box){
  if(box.nextElementSibling?.classList.contains('corr')) return;
  box.querySelector('.badBtn').classList.add('active');
  const form=document.createElement('div');
  form.className='corr';
  form.innerHTML='<label>JUSTIFICATIVA / CORRECAO</label><textarea class="textarea" rows="4" placeholder="Explique o que esta incorreto e como a resposta deveria ser."></textarea><div style="display:flex;gap:8px;margin-top:8px"><button class="btn secondary">Salvar Correcao</button><button class="btn">Cancelar</button></div>';
  box.after(form);
  const area=form.querySelector('textarea');
  const [saveBtn,cancelBtn]=form.querySelectorAll('button');
  saveBtn.onclick=async()=>{const correction=area.value.trim();if(!correction){showToast('Digite a correcao antes de salvar.');return;}await submitFeedback(query,answer,'correction',box,correction);form.remove();};
  cancelBtn.onclick=()=>form.remove();
  area.focus();
}

function autoGrow(){
  ui.userInput.style.height='auto';
  ui.userInput.style.height=Math.min(ui.userInput.scrollHeight,140)+'px';
}

function exportJson(name,data){
  const blob=new Blob([JSON.stringify(data,null,2)],{type:'application/json'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download=name;
  a.click();
}

function clearChat(){
  state.chatHistory=[];
  ui.messages.innerHTML='<div class="welcome" id="welcome"><div style="font-size:46px;opacity:.3">⚖</div><h2>Versao funcional para desktop</h2><p>Envie documentos locais, configure a chave da API e faca perguntas com memoria local, repertorio validado e recuperacao RAG.</p><div class="hints"><div class="hint" data-hint="Esta norma e aplicavel ao escopo de Meio Ambiente? Analise detalhadamente e justifique.">Aplicabilidade ao escopo de MA</div><div class="hint" data-hint="Esta norma deve ser cadastrada no CAL ou encaminhada ao CAL de Exclusoes? Justifique.">CAL ou CAL de Exclusoes</div><div class="hint" data-hint="Analise a aplicabilidade nos escopos MA, SST e RS com justificativa tecnica.">Analise MA, SST e RS</div><div class="hint" data-hint="Existe norma similar ou conflitante no repertorio validado pela equipe?">Verificar repertorio validado</div></div></div>';
  bindHints();
}

async function clearBase(){
  if(!confirm('Deseja apagar todos os documentos da base?')) return;
  for(const d of state.docs) await dbDelete(STORE_DOCS,d.id);
  state.docs=[]; rebuildIndexes(); renderAll(); el('analyzeDocsBtn').disabled=true; showToast('Base documental limpa.');
}

async function clearHistory(){
  if(!confirm('Deseja apagar o historico?')) return;
  state.history=[]; state.chatHistory=[]; await saveMemory(); rebuildIndexes(); renderAll(); clearChat(); showToast('Historico limpo.');
}

async function clearRep(){
  if(!confirm('Deseja apagar o repertorio?')) return;
  state.repertoire=[]; await saveMemory(); rebuildIndexes(); renderAll(); showToast('Repertorio limpo.');
}

function switchTab(name){
  document.querySelectorAll('.tab').forEach(t=>t.classList.toggle('active',t.dataset.tab===name));
  document.querySelectorAll('.panel').forEach(p=>p.classList.toggle('active',p.id===`panel-${name}`));
}

function bindHints(){
  document.querySelectorAll('.hint').forEach(h=>h.onclick=()=>{ui.userInput.value=h.dataset.hint||'';autoGrow();ui.userInput.focus();});
}

function bindDynamic(){
  document.querySelectorAll('[data-del-doc]').forEach(b=>b.onclick=async()=>{state.docs=state.docs.filter(d=>d.id!==b.dataset.delDoc);await dbDelete(STORE_DOCS,b.dataset.delDoc);rebuildIndexes();renderAll();el('analyzeDocsBtn').disabled=state.docs.length===0;showToast('Documento removido.');});
  document.querySelectorAll('[data-replay]').forEach(card=>card.onclick=()=>{const item=state.history.find(h=>h.id===card.dataset.replay);if(item){ui.userInput.value=item.query;autoGrow();switchTab('base');ui.userInput.focus();}});
  document.querySelectorAll('[data-toggle]').forEach(head=>head.onclick=()=>{const body=el('src-'+head.dataset.toggle);const open=body.classList.toggle('open');head.textContent=`FONTES RECUPERADAS (${body.children.length}) ${open?'▴':'▾'}`;});
}

async function analyzeDocs(){
  if(!state.docs.length) return;
  ui.userInput.value=`Analise os documentos indexados na base (${state.docs.map(d=>d.name).join(', ')}) e:
1. Identifique o tipo de norma ou ato.
2. Classifique a aplicabilidade para os escopos relevantes.
3. Aponte as principais obrigacoes legais.
4. Oriente como proceder no sistema CAL.
5. Destaque pontos de atencao, vacatio legis e hipoteses de incidencia.`;
  autoGrow();
  ui.userInput.focus();
}

async function testApi(){
  try{
    saveSettings();
    await callGemini([{role:'user',content:'Responda apenas: conexao ok'}]);
    showToast('Conexao com a API realizada com sucesso.');
  }catch(err){showToast('Falha no teste da API: '+err.message);}
}

function attachEvents(){
  document.querySelectorAll('.tab').forEach(t=>t.onclick=()=>switchTab(t.dataset.tab));
  bindHints();
  el('fileInput').addEventListener('change',async e=>{await handleFiles([...e.target.files]);e.target.value='';});
  const zone=el('uploadZone');
  zone.addEventListener('dragover',e=>{e.preventDefault();zone.classList.add('drag');});
  zone.addEventListener('dragleave',()=>zone.classList.remove('drag'));
  zone.addEventListener('drop',async e=>{e.preventDefault();zone.classList.remove('drag');await handleFiles([...e.dataTransfer.files]);});
  ui.userInput.addEventListener('keydown',e=>{if(e.key==='Enter'&&!e.shiftKey){e.preventDefault();sendMessage();}});
  ui.userInput.addEventListener('input',autoGrow);
  ui.sendBtn.addEventListener('click',sendMessage);
  el('saveConfigBtn').addEventListener('click',()=>{saveSettings();showToast('Configuracoes salvas.');});
  el('testApiBtn').addEventListener('click',testApi);
  el('exportBackupBtn').addEventListener('click',()=>exportJson(`irl_backup_${fdate()}.json`,{settings:JSON.parse(localStorage.getItem('irl-settings')||'{}'),docs:state.docs,repertoire:state.repertoire,history:state.history}));
  el('exportHistoryBtn').addEventListener('click',()=>exportJson(`irl_historico_${fdate()}.json`,state.history));
  el('exportRepBtn').addEventListener('click',()=>exportJson(`irl_repertorio_${fdate()}.json`,state.repertoire));
  el('clearKbBtn').addEventListener('click',clearBase);
  el('clearHistoryBtn').addEventListener('click',clearHistory);
  el('clearRepBtn').addEventListener('click',clearRep);
  el('clearChatBtn').addEventListener('click',clearChat);
  el('analyzeDocsBtn').addEventListener('click',analyzeDocs);
  ['apiKey','model','provider','systemPrompt','cfgTopK'].forEach(id=>el(id).addEventListener('change',saveSettings));
  el('cfgChunk').addEventListener('change',async()=>{saveSettings();await reprocessAllDocs();showToast('Chunks reprocessados com a nova configuracao.');});
}

async function init(){
  loadSettings();
  await loadDbData();
  renderAll();
  el('analyzeDocsBtn').disabled=state.docs.length===0;
  attachEvents();
}

init().catch(err=>{console.error(err);showToast('Falha ao iniciar a aplicacao: '+err.message);});
</script>
</body>
</html>
