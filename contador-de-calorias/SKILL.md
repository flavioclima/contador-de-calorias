---
name: "contador-de-calorias"
description: "Registra refeições (texto ou foto), calcula calorias e macros, guarda no Notion, planilha ou arquivo, e gera relatório mensal com gráficos clicáveis. Use ao lançar refeição ou pedir 'relatório mensal'."
---

# Contador de calorias e macros

Responda sempre no idioma da pessoa. Você calcula kcal, proteína, carboidrato e gordura de cada refeição, grava tudo no lugar que ela escolheu e, quando ela pedir, gera um relatório mensal em HTML.
As estimativas não substituem nutricionista ou médico. Diga isso uma vez, no início, sem insistir.

## 1. Primeira vez: configuração (faça antes de registrar qualquer coisa)

Se ainda não existe configuração salva, converse para montá-la. Pergunte uma coisa de cada vez, de forma curta.

**1.1 Onde guardar os registros.** Explique que precisa de um lugar permanente para as refeições e os alimentos frequentes, e deixe a pessoa escolher:
- **Notion** (precisa do conector do Notion ativo). Melhor para quem já usa Notion: tabela fácil de ler e editar.
- **Google Planilhas** (precisa do conector do Google Drive). Boa para quem prefere planilha.
- **Arquivo (CSV ou .xlsx)** na pasta de trabalho ou no computador da pessoa. Funciona sem conectores, mas ela precisa reenviar o arquivo se mudar de conversa ou dispositivo.
- **Só na conversa/memória.** Mais simples, porém frágil: pode perder histórico. Só ofereça avisando isso.

Se a opção escolhida precisar de um conector que não está disponível, diga qual conexão ajudaria e ofereça a alternativa de arquivo.

**1.2 Metas diárias.** Pergunte as metas de calorias, proteína, carboidrato e gordura. Se a pessoa não souber, ofereça calcular uma sugestão inicial a partir de objetivo, peso, altura, idade e atividade, e avise que é só um ponto de partida para conversar com um profissional.

**1.3 Criar a estrutura.**
- Uma tabela de registro com as colunas: `Data | Refeição | Descrição | Calorias (kcal) | Proteína (g) | Carboidrato (g) | Gordura (g)`. Use uma linha por item ou refeição.
- Um bloco com as metas no topo do registro.
- Uma página ou aba de **Alimentos frequentes** (nome, porção, kcal, proteína, carbo, gordura).
- Depois de criar, confirme com a pessoa onde ficou e guarde o endereço (memória, se disponível).

Nas conversas seguintes, leia a configuração existente e não pergunte tudo de novo.

## 2. Registrar uma refeição

A pessoa manda texto ("2 whey e 100 g de carne"), foto do prato ou foto de rótulo.

1. Identifique o alimento e a quantidade. Se a quantidade for essencial e não estiver clara, pergunte antes de lançar.
2. Prioridade das fontes: **rótulo/tabela nutricional da foto** > alimento da lista de frequentes > estimativa. Marque como "estimado" na descrição tudo que vier de foto do prato ou de estimativa sem rótulo.
3. **Carnes e cereais: cru ou cozido.** Os valores mudam muito com o peso. Se a pessoa não disser, pergunte ou assuma e escreva a suposição na descrição (ex.: "100 g, assumido já assado").
4. Anote cada item novo que a pessoa repete como candidato a **alimento frequente** e ofereça salvar.
5. Grave uma linha por refeição/item na data certa.
6. Mantenha **uma única linha de total por dia**, em negrito: "Total do dia (parcial)" para o dia em andamento. Ela é **substituída** a cada lançamento, nunca acumulada em duplicata. Quando o dia fecha ("novo dia" ou a data muda), vire a parcial em "Total do dia" e abra a nova data.
7. Permita lançar em dias passados ("no dia 30 adicione..."): insira a linha e recalcule o total daquele dia.
8. Se a pessoa corrigir um valor, atualize a linha e o total. Se ela estiver só perguntando ("isso é muito?"), responda sem alterar o registro.

**Resposta a cada lançamento** (curta, para ler no celular):
- O que foi lançado (kcal | proteína | carbo | gordura).
- O total parcial ou fechado do dia e **cada macro como % da meta**.

## 3. Relatório mensal

Gatilhos: "relatório mensal", "me dê o relatório", "gráfico do mês".

1. Leia **todos os dias** do registro (tabela completa, todos os meses).
2. Monte a lista `DAYS` no formato abaixo. Para refeições repetidas use códigos de um catálogo `C` (`"W"` etc.); as demais vão por extenso.
3. **Confira**: para cada dia, a soma das linhas deve bater com o total armazenado. Se houver diferença, corrija o dado ou avise a pessoa.
4. Gere o HTML a partir do modelo abaixo, trocando `/*DATA*/` pelo bloco de dados. O dia em andamento leva `,1` como terceiro item para aparecer como parcial.
5. Publique como artefato (página interna). Se não houver artefatos, envie o `.html` como arquivo. Diga em uma frase o que contém.

O relatório mostra: abas por mês, médias, dois gráficos de linha (calorias e proteína) com a meta tracejada, pontos fora da meta destacados, e **ao tocar num ponto** a tabela de refeições daquele dia com a refeição mais pesada marcada.

Formato dos dados:

~~~js
const GOAL={k:2000,p:170,c:185,f:65};            // metas da pessoa
const C={W:["Whey Chocolate",135,15,13,3]};       // [nome,kcal,prot,carb,gord]
const DAYS=[
 ["2026-11-07",["W",["Coxão mole 230g cru",414,70,0,13]]],
 ["2026-11-08",["W"],1]   // 3º item 1 = dia parcial
];
~~~

Modelo do HTML:

~~~html
<!DOCTYPE html><html lang="pt-BR"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width, initial-scale=1"><title>Relatório mensal de calorias e proteína</title>
<style>
:root{--bg:#faf9f6;--card:#fff;--ink:#1f1e1b;--mut:#6b6860;--line:#e6e3dc;--ok:#2f8f5b;--bad:#d4452f;--warn:#d98a14;--acc:#3b6fd4}
@media (prefers-color-scheme:dark){:root{--bg:#161614;--card:#201f1c;--ink:#f1efe9;--mut:#a29e94;--line:#34322d;--ok:#4fc283;--bad:#ff6b52;--warn:#f0a93a;--acc:#7aa2f0}}
*{box-sizing:border-box}body{margin:0;background:var(--bg);color:var(--ink);font:15px/1.45 system-ui,-apple-system,"Segoe UI",sans-serif;padding:16px;max-width:760px;margin-inline:auto}
h1{font-size:20px;margin:0 0 2px}.sub{color:var(--mut);font-size:13px;margin-bottom:12px}
.tabs{display:flex;gap:8px;margin-bottom:14px;flex-wrap:wrap}.tabs button{border:1px solid var(--line);background:var(--card);color:var(--ink);padding:8px 14px;border-radius:999px;font:inherit;cursor:pointer}.tabs button[aria-pressed=true]{background:var(--ink);color:var(--bg);border-color:var(--ink)}
.stats{display:grid;grid-template-columns:repeat(2,1fr);gap:8px;margin-bottom:14px}.stat{background:var(--card);border:1px solid var(--line);border-radius:12px;padding:10px 12px}.stat b{display:block;font-size:20px}.stat span{color:var(--mut);font-size:12px}
.card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:12px;margin-bottom:14px}.card h2{font-size:15px;margin:0 0 2px}
.legend{display:flex;flex-wrap:wrap;gap:10px;color:var(--mut);font-size:12px;margin-bottom:6px}.legend i{display:inline-block;width:10px;height:10px;border-radius:50%;margin-right:4px}
svg{width:100%;height:auto;display:block;touch-action:manipulation;cursor:pointer}svg text{fill:var(--mut);font-size:10px;font-family:inherit}
.nav{display:flex;align-items:center;justify-content:space-between;gap:8px;margin-bottom:8px}.nav button{border:1px solid var(--line);background:var(--bg);color:var(--ink);border-radius:8px;padding:6px 12px;font:inherit;cursor:pointer}.nav h2{margin:0;font-size:16px;text-align:center;flex:1}
.pills{display:grid;grid-template-columns:repeat(4,1fr);gap:6px;margin-bottom:10px}.pill{border:1px solid var(--line);border-radius:10px;padding:6px;text-align:center;font-size:12px;color:var(--mut)}.pill b{display:block;font-size:15px;color:var(--ink)}.pill.bad b{color:var(--bad)}.pill.ok b{color:var(--ok)}
table{width:100%;border-collapse:collapse;font-size:13px}th{font-weight:600;color:var(--mut);text-align:right;padding:4px;border-bottom:1px solid var(--line);font-size:11px}th:first-child,td:first-child{text-align:left}td{padding:6px 4px;border-bottom:1px solid var(--line);text-align:right;vertical-align:top}td:first-child{width:46%}
tr.top td{background:color-mix(in srgb,var(--bad) 12%,transparent);font-weight:600}.bar{height:4px;border-radius:2px;background:var(--acc);margin-top:3px;opacity:.55}tr.top .bar{background:var(--bad);opacity:.9}tfoot td{font-weight:700;border-bottom:0}.hint{color:var(--mut);font-size:12px;margin-top:8px}
</style></head><body>
<h1>Relatório mensal</h1><div class="sub" id="meta"></div><div class="tabs" id="tabs"></div><div class="stats" id="stats"></div>
<div class="card"><h2>Calorias por dia</h2><div class="legend"><span><i style="background:var(--ok)"></i>dentro da meta</span><span><i style="background:var(--bad)"></i>passou da meta</span><span>tracejado = meta</span></div><svg id="c1" role="img" aria-label="Calorias por dia"></svg></div>
<div class="card"><h2>Proteína por dia</h2><div class="legend"><span><i style="background:var(--ok)"></i>bateu a meta</span><span><i style="background:var(--warn)"></i>abaixo da meta</span><span>tracejado = meta</span></div><svg id="c2" role="img" aria-label="Proteína por dia"></svg></div>
<div class="card"><div class="nav"><button id="prev" aria-label="Dia anterior">‹</button><h2 id="dtitle"></h2><button id="next" aria-label="Próximo dia">›</button></div><div class="pills" id="pills"></div>
<table><thead><tr><th>Refeição</th><th>kcal</th><th>Prot</th><th>Carb</th><th>Gord</th></tr></thead><tbody id="tbody"></tbody><tfoot id="tfoot"></tfoot></table>
<div class="hint">A linha em vermelho é a que mais pesou nas calorias do dia. Toque num ponto dos gráficos para trocar de dia.</div></div>
<script>
/*DATA*/
const WD=["dom","seg","ter","qua","qui","sex","sáb"];
const days=DAYS.map(([d,ms,pt])=>{const items=ms.map(r=>typeof r==="string"?C[r]:r);const t=[0,0,0,0];items.forEach(x=>{for(let i=0;i<4;i++)t[i]+=x[i+1]});const dt=new Date(d+"T12:00:00");return{d,ym:d.slice(0,7),mes:d.slice(5,7),dia:d.slice(8),wd:WD[dt.getDay()],items,k:t[0],p:t[1],c:t[2],f:t[3],partial:!!pt,nome:dt.toLocaleDateString("pt-BR",{month:"long"})+" "+d.slice(0,4)}});
const months=[...new Set(days.map(x=>x.ym))];let ym=months[months.length-1],sel=null;
const $=id=>document.getElementById(id),fmt=n=>Math.round(n).toLocaleString("pt-BR"),cur=()=>days.filter(x=>x.ym===ym);
$("meta").textContent="Meta diária: "+GOAL.k+" kcal · "+GOAL.p+" g de proteína";
function tabs(){$("tabs").innerHTML=months.map(m=>`<button aria-pressed="${m===ym}" data-m="${m}">${days.find(x=>x.ym===m).nome}</button>`).join("");$("tabs").querySelectorAll("button").forEach(b=>b.onclick=()=>{ym=b.dataset.m;sel=cur().length-1;render()})}
function stats(){const closed=cur().filter(x=>!x.partial),n=closed.length||1;const aK=closed.reduce((a,x)=>a+x.k,0)/n,aP=closed.reduce((a,x)=>a+x.p,0)/n;const over=closed.filter(x=>x.k>GOAL.k).length,hit=closed.filter(x=>x.p>=GOAL.p).length;
$("stats").innerHTML=[[fmt(aK)+" kcal","média diária de calorias"],[over+" de "+closed.length,"dias acima de "+GOAL.k+" kcal"],[fmt(aP)+" g","média diária de proteína"],[hit+" de "+closed.length,"dias com proteína ≥ "+GOAL.p+" g"]].map(([a,b])=>`<div class="stat"><b>${a}</b><span>${b}</span></div>`).join("")}
function chart(id,key,goal,step,badFn,badColor){const ds=cur(),vals=ds.map(d=>d[key]);const lo=Math.floor((Math.min(...vals,goal)-step*.3)/step)*step,hi=Math.ceil((Math.max(...vals,goal)+step*.2)/step)*step;
const W=700,H=300,L=44,R=14,T=14,B=30;const x=i=>ds.length===1?W/2:L+(W-L-R)*i/(ds.length-1),y=v=>T+(H-T-B)*(1-(v-lo)/(hi-lo));let s="";
for(let v=lo;v<=hi;v+=step)s+=`<line x1="${L}" x2="${W-R}" y1="${y(v)}" y2="${y(v)}" stroke="var(--line)"/><text x="${L-6}" y="${y(v)+3}" text-anchor="end">${fmt(v)}</text>`;
s+=`<line x1="${L}" x2="${W-R}" y1="${y(goal)}" y2="${y(goal)}" stroke="var(--mut)" stroke-dasharray="5 4" stroke-width="1.5"/>`;
s+=`<polyline fill="none" stroke="var(--acc)" stroke-width="2" stroke-linejoin="round" points="${ds.map((d,i)=>x(i)+","+y(d[key])).join(" ")}"/>`;
const ev=ds.length>20?3:ds.length>12?2:1;
ds.forEach((d,i)=>{const bad=badFn(d[key]),col=bad?badColor:"var(--ok)",on=i===sel;if(on)s+=`<line x1="${x(i)}" x2="${x(i)}" y1="${T}" y2="${H-B}" stroke="var(--ink)" stroke-opacity=".25"/>`;
s+=`<circle cx="${x(i)}" cy="${y(d[key])}" r="${on?8:bad?5.5:4}" fill="${col}" stroke="${on?"var(--ink)":"var(--card)"}" stroke-width="${on?2.5:1.5}"${d.partial?' fill-opacity=".45"':""}/>`;
if(i%ev===0||i===ds.length-1||on)s+=`<text x="${x(i)}" y="${H-12}" text-anchor="middle"${on?' style="fill:var(--ink);font-weight:700"':""}>${d.dia}</text>`});
const el=$(id);el.setAttribute("viewBox",`0 0 ${W} ${H}`);el.innerHTML=s;
el.onclick=e=>{const r=el.getBoundingClientRect(),px=(e.clientX-r.left)/r.width*W;let best=0,bd=1e9;ds.forEach((_,i)=>{const dd=Math.abs(x(i)-px);if(dd<bd){bd=dd;best=i}});sel=best;render()}}
function detail(){const ds=cur(),d=ds[sel];$("dtitle").textContent=`${d.wd}, ${d.dia}/${d.mes}`+(d.partial?" (parcial)":"");$("prev").disabled=sel===0;$("next").disabled=sel===ds.length-1;
const pk=d.k>GOAL.k?"bad":"ok",pp=d.p>=GOAL.p?"ok":"bad";
$("pills").innerHTML=`<div class="pill ${pk}"><b>${fmt(d.k)}</b>kcal · ${Math.round(d.k/GOAL.k*100)}%</div><div class="pill ${pp}"><b>${fmt(d.p)} g</b>prot · ${Math.round(d.p/GOAL.p*100)}%</div><div class="pill"><b>${fmt(d.c)} g</b>carb · ${Math.round(d.c/GOAL.c*100)}%</div><div class="pill"><b>${fmt(d.f)} g</b>gord · ${Math.round(d.f/GOAL.f*100)}%</div>`;
const max=Math.max(...d.items.map(x=>x[1]));$("tbody").innerHTML=d.items.map(x=>`<tr${x[1]===max?' class="top"':""}><td>${x[0]}<div class="bar" style="width:${Math.round(x[1]/max*100)}%"></div></td><td>${x[1]}</td><td>${x[2]}</td><td>${x[3]}</td><td>${x[4]}</td></tr>`).join("");
$("tfoot").innerHTML=`<tr><td>Total</td><td>${fmt(d.k)}</td><td>${d.p}</td><td>${d.c}</td><td>${d.f}</td></tr>`}
function render(){const ds=cur();if(sel===null||sel>=ds.length)sel=ds.length-1;tabs();stats();chart("c1","k",GOAL.k,500,v=>v>GOAL.k,"var(--bad)");chart("c2","p",GOAL.p,40,v=>v<GOAL.p,"var(--warn)");detail()}
$("prev").onclick=()=>{if(sel>0){sel--;render()}};$("next").onclick=()=>{if(sel<cur().length-1){sel++;render()}};render();
</script></body></html>
~~~

## 4. Cuidados

- Não invente dados: se faltar informação para um dia, mostre o dia como está e avise.
- Não dê conselho médico nem prometa resultado de emagrecimento. Se a pessoa tiver condição de saúde (como diabetes), sugira alinhar metas com o profissional dela.
- Se a pessoa descrever comportamento alimentar preocupante (jejuns extremos, culpa intensa, compensações), pare de otimizar números e responda com cuidado, sugerindo apoio profissional.
- Mantenha respostas curtas: o que foi lançado, o total do dia e as porcentagens.