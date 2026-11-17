[hospice-narrative-app.html](https://github.com/user-attachments/files/34946663/hospice-narrative-app.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Hospice Nursing Narrative Builder</title>
<style>
  :root{
    --navy:#1f4e78; --navy-dark:#163a5a; --blue:#d9eaf7; --yellow:#fff8e1;
    --green:#e8f5e9; --gray:#eceff1; --border:#c8d2dc; --text:#1a2733;
    --muted:#5b6b7c; --red:#c0392b;
  }
  *{box-sizing:border-box;}
  body{
    margin:0; font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
    color:var(--text); background:#f4f7fa; line-height:1.5;
  }
  header{
    background:var(--navy); color:#fff; padding:18px 24px;
  }
  header h1{margin:0; font-size:20px; letter-spacing:.2px;}
  header p{margin:4px 0 0; font-size:13px; opacity:.85;}
  .wrap{max-width:1180px; margin:0 auto; padding:20px 24px 60px;}
  .banner{
    background:#fff3cd; border:1px solid #ffe08a; border-left:5px solid #d9a300;
    padding:12px 16px; border-radius:6px; margin-bottom:18px; font-size:13.5px;
  }
  .banner strong{display:block; margin-bottom:3px;}
  .panel{
    background:#fff; border:1px solid var(--border); border-radius:8px;
    padding:16px 18px; margin-bottom:18px;
  }
  .panel h2{
    margin:0 0 10px; font-size:15px; text-transform:uppercase; letter-spacing:.6px;
    color:var(--navy);
  }
  .bar-head{display:flex; align-items:center; gap:14px; flex-wrap:wrap;}
  .bar{
    flex:1 1 320px; height:26px; background:var(--gray); border-radius:13px; overflow:hidden;
    border:1px solid var(--border);
  }
  .bar > div{
    height:100%; width:0%; background:linear-gradient(90deg,#4a90c2,#1f7a4d);
    transition:width .18s ease;
  }
  .pct{font-size:26px; font-weight:700; color:var(--navy); min-width:78px; text-align:right;}
  .counts{font-size:13px; color:var(--muted);}
  table{width:100%; border-collapse:collapse; font-size:14px;}
  th{
    background:var(--navy); color:#fff; text-align:left; padding:9px 10px;
    font-size:12px; text-transform:uppercase; letter-spacing:.5px;
  }
  td{border-bottom:1px solid #e3e9ee; padding:7px 10px; vertical-align:top;}
  tr.done td{background:#f1f8f2;}
  td.crit{font-weight:600; background:var(--blue); width:22%;}
  td.chk{width:88px; text-align:center;}
  td.chk input{width:19px; height:19px; cursor:pointer; accent-color:#1f7a4d;}
  td.det, td.pv{width:39%;}
  input[type=text], textarea{
    width:100%; padding:7px 9px; border:1px solid var(--border); border-radius:5px;
    font:inherit; font-size:13.5px; background:var(--yellow);
  }
  input[type=text]:focus, textarea:focus{
    outline:2px solid #7fb2d9; outline-offset:1px; background:#fffdf3;
  }
  .pv{
    font-size:13px; color:#33566f; background:var(--green); border-radius:5px;
    padding:7px 9px; min-height:44px;
  }
  .pv.empty{
    color:#9aa7b3; background:#fafbfc; border:1px dashed #dbe3ea; font-style:italic;
  }
  textarea{
    min-height:150px; background:#fff; font-size:14px; line-height:1.6; resize:vertical;
  }
  .actions{display:flex; gap:10px; flex-wrap:wrap; margin-top:12px;}
  button{
    font:inherit; font-size:13.5px; font-weight:600; padding:9px 16px; border-radius:6px;
    border:1px solid var(--navy); background:var(--navy); color:#fff; cursor:pointer;
  }
  button.ghost{background:#fff; color:var(--navy);}
  button.danger{background:#fff; color:var(--red); border-color:#e5b4ad;}
  button:hover{opacity:.9;}
  button:disabled{opacity:.45; cursor:not-allowed;}
  .toast{
    position:fixed; left:50%; bottom:26px; transform:translateX(-50%) translateY(80px);
    background:#1f7a4d; color:#fff; padding:10px 18px; border-radius:6px; font-size:14px;
    opacity:0; transition:all .25s ease; pointer-events:none; z-index:50;
  }
  .toast.show{opacity:1; transform:translateX(-50%) translateY(0);}
  .fineprint{font-size:12.5px; color:var(--muted); margin-top:10px;}
  ol.notes{font-size:13.5px; margin:8px 0 0; padding-left:20px;}
  ol.notes li{margin-bottom:5px;}
  @media print{
    header,.banner,.actions,button,.toast,.noprint{display:none !important;}
    body{background:#fff;}
    .panel{border:none; padding:0; margin-bottom:14px;}
    .bar{display:none;}
    textarea{border:none; min-height:60px; height:auto;}
    table{font-size:11.5px;}
  }
</style>
</head>
<body>
<header>
  <h1>Hospice Nursing Narrative Builder</h1>
  <p>Enter details, check off completed items, copy the finished narrative.</p>
</header>

<div class="wrap">

  <div class="banner noprint">
    <strong>Privacy notice — read before entering anything.</strong>
    Nothing typed here is saved, transmitted or stored. This page holds all information in
    the browser tab's memory only. Closing or refreshing the tab, or pressing
    <em>Clear all</em>, erases everything permanently. Print or copy the note out before you
    close the tab. Do not use this tool for protected health information unless your agency
    has approved this workflow.
  </div>

  <div class="panel noprint">
    <h2>Data completeness</h2>
    <div class="bar-head">
      <div class="bar"><div id="barFill"></div></div>
      <div class="pct" id="pctText">0%</div>
    </div>
    <div class="counts" id="countText">0 of 18 items checked</div>
    <p class="fineprint">The percentage counts how many of the 18 criteria you have checked in
    the <strong>Noted?</strong> column. Checking a box is a confirmation by you that the item is
    documented — it is not automatic. Entering a detail and checking the box are independent
    actions.</p>
  </div>

  <div class="panel">
    <h2>Narrative builder</h2>
    <table>
      <thead>
        <tr>
          <th>Data criteria</th>
          <th>Noted?</th>
          <th>Details</th>
          <th>Narrative sentence</th>
        </tr>
      </thead>
      <tbody id="rows"></tbody>
    </table>
  </div>

  <div class="panel">
    <h2>Completed narrative note</h2>
    <textarea id="note" readonly placeholder="Enter information in the Details column to generate the note."></textarea>
    <div class="actions noprint">
      <button id="copyBtn">Copy note</button>
      <button id="printBtn" class="ghost">Print / save as PDF</button>
      <button id="clearBtn" class="danger">Clear all</button>
    </div>
    <p class="fineprint">The note is generated live from the sentences above, joined in order.
    Review and edit it for accuracy, clinical context, agency policy and required documentation
    before you sign it. Add units, qualifiers and relevant negatives in the Details column where
    they matter.</p>
  </div>

  <div class="panel noprint">
    <h2>How to use this</h2>
    <ol class="notes">
      <li>Enter the patient-specific finding for each criterion in the yellow <strong>Details</strong> cell.</li>
      <li>Confirm the item is documented in your charting system, then tick its <strong>Noted?</strong> box.</li>
      <li>Watch the completeness bar at the top — it reaches 100% when all 18 are checked.</li>
      <li>Copy the finished narrative into the patient's record. Print or save as PDF if you need a paper copy.</li>
      <li>Press <strong>Clear all</strong> when you are finished. Closing the tab also clears everything.</li>
    </ol>
  </div>

</div>

<div class="toast" id="toast">Copied to clipboard</div>

<script>
(function(){
  "use strict";

  var CRITERIA = [
    ["PPS SCORE",            "Patient has a PPS score of {d}."],
    ["FAST SCORE",           "Patient has a FAST score of {d}."],
    ["MUAC",                 "Patient MUAC is {d}."],
    ["NYHA score",           "Patient has an NYHA score of {d}."],
    ["WEIGHT",               "Patient weight is {d}."],
    ["VITALS",               "Patient vital signs are {d}."],
    ["PAIN",                 "Patient pain assessment is {d}."],
    ["SKIN",                 "Patient skin assessment reveals {d}."],
    ["INFECTION",            "Patient infection status is {d}."],
    ["ADLS",                 "Patient ADL status is {d}."],
    ["AMBULATION",           "Patient ambulation status is {d}."],
    ["COGNITIVE STATUS",     "Patient cognitive status is {d}."],
    ["COMMUNICATION",        "Patient communication status is {d}."],
    ["MEDICATIONS",          "Patient medication review indicates {d}."],
    ["SLEEP",                "Patient sleep pattern is {d}."],
    ["INTAKE",               "Patient intake is {d}."],
    ["SYMPTOMS/COMPLAINTS",  "Patient symptoms and complaints include {d}."],
    ["PRIMARY DX FOCUS",     "Patient primary diagnosis focus is {d}."]
  ];

  var tbody   = document.getElementById("rows");
  var note    = document.getElementById("note");
  var barFill = document.getElementById("barFill");
  var pctText = document.getElementById("pctText");
  var countText = document.getElementById("countText");
  var toast   = document.getElementById("toast");

  var state = CRITERIA.map(function(){ return { detail: "", checked: false }; });

  function buildRows(){
    CRITERIA.forEach(function(row, i){
      var tr = document.createElement("tr");

      var tdCrit = document.createElement("td");
      tdCrit.className = "crit";
      tdCrit.textContent = row[0];

      var tdChk = document.createElement("td");
      tdChk.className = "chk";
      var box = document.createElement("input");
      box.type = "checkbox";
      box.id = "chk" + i;
      box.setAttribute("aria-label", "Noted: " + row[0]);
      box.addEventListener("change", function(){
        state[i].checked = box.checked;
        render();
      });
      tdChk.appendChild(box);

      var tdDet = document.createElement("td");
      tdDet.className = "det";
      var inp = document.createElement("input");
      inp.type = "text";
      inp.id = "det" + i;
      inp.placeholder = "Enter " + row[0].toLowerCase() + " detail";
      inp.setAttribute("aria-label", "Details: " + row[0]);
      inp.addEventListener("input", function(){
        state[i].detail = inp.value;
        render();
      });
      tdDet.appendChild(inp);

      var tdPv = document.createElement("td");
      tdPv.className = "pv empty";
      tdPv.id = "pv" + i;
      tdPv.textContent = "Awaiting detail";

      tr.appendChild(tdCrit);
      tr.appendChild(tdChk);
      tr.appendChild(tdDet);
      tr.appendChild(tdPv);
      tr.id = "row" + i;
      tbody.appendChild(tr);
    });
  }

  function sentence(item, detail){
    var trimmed = (detail || "").trim();
    if(!trimmed) return "";
    return item[1].replace("{d}", trimmed);
  }

  function render(){
    var sentences = [];
    var checked = 0;

    CRITERIA.forEach(function(item, i){
      var st = state[i];
      var pv = document.getElementById("pv" + i);
      var s  = sentence(item, st.detail);

      if(s){
        pv.textContent = s;
        pv.className = "pv";
        sentences.push(s);
      } else {
        pv.textContent = "Awaiting detail";
        pv.className = "pv empty";
      }

      if(st.checked){
        checked++;
        document.getElementById("row" + i).classList.add("done");
      } else {
        document.getElementById("row" + i).classList.remove("done");
      }
    });

    note.value = sentences.length
      ? sentences.join(" ")
      : "";

    var pct = Math.round((checked / CRITERIA.length) * 100);
    barFill.style.width = pct + "%";
    pctText.textContent = pct + "%";
    countText.textContent = checked + " of " + CRITERIA.length + " items checked";
  }

  function showToast(msg){
    toast.textContent = msg;
    toast.classList.add("show");
    setTimeout(function(){ toast.classList.remove("show"); }, 1800);
  }

  document.getElementById("copyBtn").addEventListener("click", function(){
    var text = note.value;
    if(!text){ showToast("Nothing to copy yet"); return; }

    function done(){ showToast("Copied to clipboard"); }
    function fallback(){
      note.removeAttribute("readonly");
      note.select();
      try {
        var ok = document.execCommand("copy");
        showToast(ok ? "Copied to clipboard" : "Press Ctrl+C / Cmd+C to copy");
      } catch(e){
        showToast("Press Ctrl+C / Cmd+C to copy");
      }
      note.setAttribute("readonly","readonly");
      window.getSelection().removeAllRanges();
    }

    if(navigator.clipboard && navigator.clipboard.writeText){
      navigator.clipboard.writeText(text).then(done, fallback);
    } else {
      fallback();
    }
  });

  document.getElementById("printBtn").addEventListener("click", function(){
    window.print();
  });

  document.getElementById("clearBtn").addEventListener("click", function(){
    if(!window.confirm("Clear every field and checkbox? This cannot be undone.")) return;
    state = CRITERIA.map(function(){ return { detail: "", checked: false }; });
    CRITERIA.forEach(function(item, i){
      document.getElementById("det" + i).value = "";
      document.getElementById("chk" + i).checked = false;
      document.getElementById("det" + i).blur();
    });
    render();
    showToast("Cleared");
  });

  buildRows();
  render();
})();
</script>
</body>
</html>
