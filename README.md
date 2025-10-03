<!-- DYNAMIC ODDS WIDGET -->
<div id="limitless-odds-root" style="position:fixed;right:20px;bottom:20px;z-index:999999;font-family:Inter,system-ui,Arial;">
  <style>
    #limitless-odds-root .lw-panel { width:340px; max-height:70vh; overflow:auto;background:rgba(0,0,0,0.9); color:#fff; border-radius:12px; padding:12px; box-shadow:0 10px 30px rgba(0,0,0,0.5);}
    #limitless-odds-root h4{margin:0 0 10px 0;font-size:15px}
    #limitless-odds-root .q{margin-bottom:12px;padding:10px;background:rgba(255,255,255,0.05);border-radius:8px}
    #limitless-odds-root .title{margin:0 0 6px 0;font-size:13px}
    #limitless-odds-root .odds{display:flex;justify-content:space-between;gap:6px}
    #limitless-odds-root button{flex:1;border:0;padding:6px 10px;border-radius:8px;cursor:pointer;color:#fff}
    #limitless-odds-root .yes{background:#28a745;}
    #limitless-odds-root .no{background:#dc3545;}
  </style>

  <div class="lw-panel" id="lw-panel">
    <h4>🔥 Limitless Funny Market</h4>
    <div id="lw-questions"></div>
  </div>
</div>

<script>
(function(){
  const KEY = 'limitless_dynamic_odds_v1';

  // Initial questions + odds
  const questions = [
    {
      text: "Will Limitless pull 1B+ FDV at TGE?",
      yes: 50, no: 50
    },
    {
      text: "Who has major bald arc — CJ or Heisenberg?",
      yes: 50, no: 50
    },
    {
      text: "Will Limitless hit 1B+ volume before TGE?",
      yes: 20, no: 80
    }
  ];

  // Load saved state
  let state = JSON.parse(localStorage.getItem(KEY) || 'null');
  if(!state) { state = questions; save(); }

  function save(){ localStorage.setItem(KEY, JSON.stringify(state)); }

  function render(){
    const root = document.getElementById('lw-questions');
    root.innerHTML = '';
    state.forEach((q, i) => {
      const box = document.createElement('div');
      box.className='q';

      const title = document.createElement('div');
      title.className='title';
      title.textContent = q.text;
      box.appendChild(title);

      const odds = document.createElement('div');
      odds.className='odds';

      const yesBtn = document.createElement('button');
      yesBtn.className='yes';
      yesBtn.textContent = `Yes ${q.yes}%`;
      yesBtn.onclick = () => vote(i, 'yes');

      const noBtn = document.createElement('button');
      noBtn.className='no';
      noBtn.textContent = `No ${q.no}%`;
      noBtn.onclick = () => vote(i, 'no');

      odds.appendChild(yesBtn);
      odds.appendChild(noBtn);
      box.appendChild(odds);

      root.appendChild(box);
    });
  }

  function vote(index, side){
    let q = state[index];
    if(side==='yes'){
      q.yes = Math.min(100, q.yes + 2);
      q.no = Math.max(0, 100 - q.yes);
    } else {
      q.no = Math.min(100, q.no + 2);
      q.yes = Math.max(0, 100 - q.no);
    }
    save();
    render();
  }

  render();
})();
</script>
<!-- END WIDGET -->