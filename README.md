<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Duplicate Code Checker with Copy</title>
<style>
  body{font-family:sans-serif;background:#f7fafc;margin:0;padding:20px}
  textarea{width:100%;height:200px;margin-bottom:8px;font-family:monospace;font-size:13px}
  button{margin:4px;padding:6px 12px;cursor:pointer}
  .results{display:flex;gap:10px;flex-wrap:wrap;margin-top:12px}
  .result-box{background:#fff;padding:8px;border:1px solid #ccc;border-radius:6px;flex:1;min-width:200px;max-height:400px;overflow:auto}
</style>
</head>
<body>
<h2>Duplicate Code Checker</h2>
<p>Set 1</p>
<textarea id="set1"></textarea>
<p>Set 2</p>
<textarea id="set2"></textarea>

<p>Add new codes</p>
<textarea id="newCodes"></textarea>
<br>
<button id="addToSet1">Add to Set 1</button>
<button id="addToSet2">Add to Set 2</button>
<button id="check">Check duplicates</button>
<button id="clearAll">Clear all</button>

<div class="results">
  <div class="result-box">
    <strong>Duplicates</strong>
    <div id="duplicates"></div>
    <button onclick="copyText('duplicates')">Copy</button>
  </div>
  <div class="result-box">
    <strong>Only in Set 1</strong>
    <div id="only1"></div>
    <button onclick="copyText('only1')">Copy</button>
  </div>
  <div class="result-box">
    <strong>Only in Set 2</strong>
    <div id="only2"></div>
    <button onclick="copyText('only2')">Copy</button>
  </div>
</div>

<script>
function parseCodes(el){ return el.value.split(/\r?\n/).map(s=>s.trim().toUpperCase()).filter(s=>s!==""); }
function listToHtml(list){ return list.length ? '<ol>'+list.map(x=>'<li>'+x+'</li>').join('')+'</ol>' : '<i>— none —</i>'; }

function checkDuplicates(){
  const a=parseCodes(document.getElementById('set1'));
  const b=parseCodes(document.getElementById('set2'));
  const sa=new Set(a), sb=new Set(b);
  const dup=[...sa].filter(x=>sb.has(x)).sort();
  const onlyA=[...sa].filter(x=>!sb.has(x)).sort();
  const onlyB=[...sb].filter(x=>!sa.has(x)).sort();
  document.getElementById('duplicates').innerHTML=listToHtml(dup);
  document.getElementById('only1').innerHTML=listToHtml(onlyA);
  document.getElementById('only2').innerHTML=listToHtml(onlyB);
}

document.getElementById('check').onclick=checkDuplicates;

document.getElementById('addToSet1').onclick=()=>{
  const codes=parseCodes(document.getElementById('newCodes'));
  if(!codes.length){ alert('No codes!'); return; }
  document.getElementById('set1').value += (document.getElementById('set1').value?"\n":"") + codes.join("\n");
  document.getElementById('newCodes').value='';
  checkDuplicates();
};

document.getElementById('addToSet2').onclick=()=>{
  const codes=parseCodes(document.getElementById('newCodes'));
  if(!codes.length){ alert('No codes!'); return; }
  document.getElementById('set2').value += (document.getElementById('set2').value?"\n":"") + codes.join("\n");
  document.getElementById('newCodes').value='';
  checkDuplicates();
};

document.getElementById('clearAll').onclick=()=>{
  document.getElementById('set1').value='';
  document.getElementById('set2').value='';
  document.getElementById('newCodes').value='';
  document.getElementById('duplicates').innerHTML='';
  document.getElementById('only1').innerHTML='';
  document.getElementById('only2').innerHTML='';
};

function copyText(id){
  const el = document.getElementById(id);
  if(!el) return;
  const temp = document.createElement('textarea');
  temp.value = el.innerText.replace(/\\n\\s*/g, '\n'); // plain text
  document.body.appendChild(temp);
  temp.select();
  document.execCommand('copy');
  document.body.removeChild(temp);
  alert('Copied ' + id + '!');
}
</script>
</body>
</html>
