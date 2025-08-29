// ...existing code...
<!doctype html>
<html lang="tr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Dijital Kart - IBAN & İsim</title>
  <style>
    body{font-family:Segoe UI,Roboto,Arial;margin:24px;background:#f5f7fb;color:#111}
    .card{background:#fff;max-width:420px;padding:28px;border-radius:10px;box-shadow:0 6px 18px rgba(0,0,0,0.08);position:relative;overflow:hidden}
    /* hafif arka plan logo */
    .card::before{
      content:"";
      position:absolute;
      inset:0;
      background:url('duygu-logo.png') center/contain no-repeat;
      opacity:0.06;
      pointer-events:none;
      z-index:0;
    }
    /* içerik üstte olsun */
    .card > *{position:relative; z-index:1}
    .row{display:flex;align-items:center;justify-content:space-between;margin:10px 0}
    .label{font-size:12px;color:#666;margin-bottom:6px}
    .value{font-weight:600;word-break:break-all}
    button.copy{background:#0366d6;border:none;color:#fff;padding:6px 10px;border-radius:6px;cursor:pointer}
    button.copy:active{transform:translateY(1px)}
    .small{font-size:13px;color:#444}
    .notice{font-size:12px;color:#666;margin-top:8px}
  </style>
</head>
<body>
  <div class="card">
    <div class="label">IBAN</div>
    <div class="row">
      <div>
        <div id="iban" class="value">TR540003200000000066913526</div>
        <div class="notice">IBAN'ı kopyalamak için butona basın.</div>
      </div>
      <div>
        <button class="copy" data-copy="iban" id="copyIban">KOPYALA</button>
      </div>
    </div>

    <div class="label">İsim</div>
    <div class="row">
      <div>
        <div id="name" class="value">GİZEM DUYGU DOĞAN ŞAHAN</div>
      </div>
      <div>
        <button class="copy" data-copy="name" id="copyName">KOPYALA</button>
      </div>
    </div>

    <!-- QR tamamen kaldırıldı; sadece küçük bir not bırakıldı -->
    <div class="notice" style="margin-top:18px">Kart: İsim ve IBAN bilgisi burada yer alır.</div>
  </div>

  <script>
    const IBAN = 'TR540003200000000066913526';
    const NAME = 'GİZEM DUYGU DOĞAN ŞAHAN';

    document.getElementById('iban').textContent = IBAN;
    document.getElementById('name').textContent = NAME;

    async function init() {
      // QR ile ilgili hiçbir kod yok
      document.getElementById('copyIban').addEventListener('click', ()=> copyText(IBAN, 'IBAN kopyalandı'));
      document.getElementById('copyName').addEventListener('click', ()=> copyText(NAME, 'İsim kopyalandı'));
    }

    async function copyText(text, successMsg) {
      try {
        await navigator.clipboard.writeText(text);
        flashBtnMessage(successMsg);
      } catch (e) {
        const ta = document.createElement('textarea');
        ta.value = text;
        document.body.appendChild(ta);
        ta.select();
        try { document.execCommand('copy'); flashBtnMessage(successMsg); } catch(_) { alert('Kopyalama başarısız'); }
        ta.remove();
      }
    }

    function flashBtnMessage(msg) {
      const old = document.title;
      document.title = msg;
      setTimeout(()=> document.title = old, 1400);
    }

    init();
  </script>
</body>
