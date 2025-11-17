<!--
Fichier: index.html
Description: Site professionnel simple pour vendre / réserver des meubles en palette.
Instructions:
 - Ouvre ce fichier dans ton navigateur (double-clic) pour tester localement.
 - Pour mettre en ligne gratuitement: utiliser GitHub Pages (déposer ce fichier dans un repo nommé e.g. username.github.io) ou Netlify/Surge.
 - Le système de réservation est local (localStorage). Pour production il faudra un backend (ex: Firebase, Supabase, ou un serveur) pour stocker et gérer les réservations.
 - Accès admin: cliquer sur "Admin" dans le menu et entrer le mot de passe: admin123 (démo).
--><!doctype html>

<html lang="fr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Meubles en palette — Réservations</title>
  <meta name="description" content="Meubles artisanaux fabriqués à partir de palettes. Réservez en ligne votre meuble préféré.">
  <link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Crect width='100' height='100' fill='%23333333'/%3E%3Ctext x='50' y='55' font-size='40' text-anchor='middle' fill='white'%3E%F0%9F%8C%B0%3C/text%3E%3C/svg%3E">
  <style>
    :root{--accent:#8B5E3C;--accent-2:#CE9A6B;--bg:#faf7f2;--card:#ffffff;--muted:#666}
    *{box-sizing:border-box}
    body{font-family:Inter, ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; margin:0; background:var(--bg); color:#222}
    header{background:linear-gradient(90deg, rgba(139,94,60,0.98), rgba(206,154,107,0.95)); color:white; padding:2rem 1rem}
    .container{max-width:1100px; margin:0 auto}
    .nav{display:flex; align-items:center; justify-content:space-between}
    .brand{font-weight:700; font-size:1.25rem}
    nav a{color:rgba(255,255,255,0.95); text-decoration:none; margin-left:1rem}
    .hero{display:grid; grid-template-columns:1fr 420px; gap:2rem; align-items:center; margin-top:1.5rem}
    .hero h1{margin:0; font-size:2rem}
    .hero p{color:rgba(255,255,255,0.95); margin:1rem 0 0}
    .cta{margin-top:1.25rem}
    .btn{background:var(--card); color:var(--accent); padding:0.6rem 1rem; border-radius:8px; border:none; cursor:pointer; font-weight:600}main{padding:2rem 1rem}
.grid{display:grid; grid-template-columns:repeat(auto-fit,minmax(260px,1fr)); gap:1.25rem}
.card{background:var(--card); border-radius:12px; box-shadow:0 6px 18px rgba(24,24,24,0.06); overflow:hidden; display:flex; flex-direction:column}
.card img{width:100%; height:200px; object-fit:cover}
.card-body{padding:1rem; flex:1; display:flex; flex-direction:column}
.price{margin-left:auto; font-weight:700; color:var(--accent)}
.meta{display:flex; gap:0.5rem; align-items:center; color:var(--muted); font-size:0.95rem}
.actions{display:flex; gap:0.5rem; margin-top:1rem}

footer{padding:1.5rem; text-align:center; color:var(--muted); background:transparent}

/* Modal */
.overlay{position:fixed; inset:0; background:rgba(0,0,0,0.5); display:none; align-items:center; justify-content:center; padding:1rem}
.modal{background:var(--card); border-radius:12px; max-width:760px; width:100%; box-shadow:0 18px 40px rgba(12,12,12,0.25); overflow:auto; max-height:90vh}
.modal .header{display:flex; align-items:center; justify-content:space-between; padding:1rem 1.25rem; border-bottom:1px solid #eee}
.modal .content{padding:1rem 1.25rem}

label{display:block; font-size:0.9rem; margin-bottom:0.25rem}
input[type=text], input[type=email], input[type=tel], textarea, select, input[type=date], input[type=number]{width:100%; padding:0.7rem; border-radius:8px; border:1px solid #e6e6e6; margin-bottom:0.75rem}
.row{display:flex; gap:0.75rem}
.row .col{flex:1}

.small{font-size:0.85rem; color:var(--muted)}

@media (max-width:900px){
  .hero{grid-template-columns:1fr}
}

  </style>
</head>
<body>
  <header>
    <div class="container nav">
      <div class="brand">Atelier Palette</div>
      <div>
        <a href="#catalog">Catalogue</a>
        <a href="#contact">Contact</a>
        <a href="#" id="open-admin" style="margin-left:1rem; font-weight:600;">Admin</a>
      </div>
    </div>
    <div class="container hero">
      <div>
        <h1>Meubles artisanaux en palette — réservation en ligne</h1>
        <p>Fauteuils, tables basses, étagères et plus — chaque pièce est fabriquée à la main à partir de palettes récupérées. Réserve en quelques clics.</p>
        <div class="cta">
          <button class="btn" id="scroll-catalog">Voir le catalogue</button>
        </div>
      </div>
      <div style="background:rgba(255,255,255,0.12); padding:1rem; border-radius:12px; color:white">
        <h3 style="margin-top:0">Comment ça marche</h3>
        <ol style="padding-left:1rem; margin:0">
          <li>Choisis un meuble dans le catalogue</li>
          <li>Choisis une date & créneau, puis réserve</li>
          <li>Nous te confirmons et préparons la pièce</li>
        </ol>
      </div>
    </div>
  </header>  <main class="container">
    <section id="catalog">
      <h2>Catalogue</h2>
      <p class="small">Clique sur "Réserver" pour réserver une pièce. Les réservations sont sauvegardées localement (démo). Pour un site "pro", je peux connecter une base de données.</p>
      <div id="products" class="grid" style="margin-top:1rem"></div>
    </section><section id="contact" style="margin-top:2rem">
  <h2>Contact</h2>
  <p class="small">Questions ? Envoie-nous un message.</p>
  <form id="contact-form" style="max-width:700px; margin-top:0.75rem">
    <label for="c-name">Nom</label>
    <input id="c-name" type="text" required>
    <label for="c-email">Email</label>
    <input id="c-email" type="email" required>
    <label for="c-message">Message</label>
    <textarea id="c-message" rows="4"></textarea>
    <div style="margin-top:0.5rem">
      <button class="btn" type="submit">Envoyer</button>
    </div>
  </form>
</section>

  </main>  <footer>
    © <span id="year"></span> Atelier Palette — Fabriqué avec soin
  </footer>  <!-- Modal reservation -->  <div id="overlay" class="overlay" role="dialog" aria-hidden="true">
    <div class="modal" role="document">
      <div class="header">
        <strong id="modal-title">Réserver</strong>
        <div style="display:flex; gap:0.5rem; align-items:center; padding-right:1rem">
          <button id="export-csv" class="btn" style="background:transparent; color:white; border:1px solid rgba(255,255,255,0.2);">Exporter (admin)</button>
          <button id="close-modal" class="btn">Fermer</button>
        </div>
      </div>
      <div class="content">
        <div id="product-summary" style="display:flex; gap:1rem; align-items:center; margin-bottom:0.5rem">
          <img id="summary-img" src="" alt="" style="width:120px; height:80px; object-fit:cover; border-radius:8px;">
          <div>
            <div id="summary-name" style="font-weight:700"></div>
            <div class="small" id="summary-desc"></div>
            <div style="margin-top:0.5rem"><span style="font-weight:700; color:var(--accent);" id="summary-price"></span></div>
          </div>
        </div><form id="reservation-form">
      <input type="hidden" id="product-id">
      <div class="row">
        <div class="col">
          <label for="res-name">Ton nom</label>
          <input id="res-name" type="text" required>
        </div>
        <div class="col">
          <label for="res-phone">Téléphone</label>
          <input id="res-phone" type="tel" required>
        </div>
      </div>
      <label for="res-email">Email</label>
      <input id="res-email" type="email" required>

      <div class="row">
        <div class="col">
          <label for="res-date">Date de retrait / réservation</label>
          <input id="res-date" type="date" required>
        </div>
        <div class="col">
          <label for="res-slot">Créneau</label>
          <select id="res-slot" required>
            <option value="matin">Matin (9h-12h)</option>
            <option value="apres-midi">Après-midi (13h-17h)</option>
            <option value="soir">Soir (18h-20h)</option>
          </select>
        </div>
      </div>

      <label for="res-qty">Quantité</label>
      <input id="res-qty" type="number" min="1" value="1" required>

      <label for="res-note">Note / personnalisation</label>
      <textarea id="res-note" rows="3" placeholder="Ex: finition cire, couleur... (optionnel)"></textarea>

      <div style="margin-top:0.5rem; display:flex; gap:0.5rem; justify-content:flex-end">
        <button class="btn" type="submit">Confirmer la réservation</button>
      </div>
    </form>

    <div id="reservation-result" style="display:none; margin-top:0.75rem"></div>
  </div>
</div>

  </div>  <!-- Admin login modal -->  <div id="admin-overlay" class="overlay" role="dialog" aria-hidden="true">
    <div class="modal" style="max-width:420px;">
      <div class="header"><strong>Admin</strong><button id="close-admin" class="btn">Fermer</button></div>
      <div class="content">
        <p class="small">Entrer le mot de passe admin pour voir les réservations (démo).</p>
        <label for="admin-pass">Mot de passe</label>
        <input id="admin-pass" type="password">
        <div style="margin-top:0.5rem; display:flex; gap:0.5rem; justify-content:flex-end">
          <button id="admin-login" class="btn">Se connecter</button>
        </div>
        <div id="admin-area" style="display:none; margin-top:0.75rem">
          <h3>Réservations</h3>
          <div id="admin-list" style="max-height:300px; overflow:auto"></div>
        </div>
      </div>
    </div>
  </div>  <script>
    // Data produits: modifie / ajoute ici.
    const PRODUCTS = [
      { id: 'p1', name: 'Table basse "Racines"', price: '120€', img: 'https://images.unsplash.com/photo-1540574163026-643ea20ade25?q=80&w=1200&auto=format&fit=crop&ixlib=rb-4.0.3&s=0f06bf3d0b0b4d0b8f7e6c6a5b0b4c9e', desc: 'Table basse en palettes, finition naturelle, idéale pour salon.' },
      { id: 'p2', name: 'Étagère murale', price: '90€', img: 'https://images.unsplash.com/photo-1550583724-b26935b657b2?q=80&w=1200&auto=format&fit=crop&ixlib=rb-4.0.3&s=6f9c2f4d5a0f4f7b8a8d3b8d9e9f6d1a', desc: 'Étagère légère pour livres et plantes.' },
      { id: 'p3', name: 'Fauteuil palette', price: '75€', img: 'https://images.unsplash.com/photo-1533779187830-7f1e6d3c1ca5?q=80&w=1200&auto=format&fit=crop&ixlib=rb-4.0.3&s=1c8f9b0d4c0e9e8b7c4a6d8a5b0c2d3f', desc: 'Fauteuil confortable avec coussin. Parfait pour terrasse.' },
      { id: 'p4', name: 'Console d'entrée', price: '95€', img: 'https://images.unsplash.com/photo-1541534401786-6a0a66a4d4a2?q=80&w=1200&auto=format&fit=crop&ixlib=rb-4.0.3&s=9f7a3b2c1d4e5f6a7b8c9d0e1f2a3b4c', desc: 'Console étroite pour entrée, finition vernie.' }
    ];

    // Utilitaires stockage
    const STORAGE_KEY = 'atelier_palette_reservations_v1';
    function loadReservations(){
      try{ return JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]'); }catch(e){return []}
    }
    function saveReservations(arr){ localStorage.setItem(STORAGE_KEY, JSON.stringify(arr)); }

    // Affichage produits
    const productsEl = document.getElementById('products');
    function renderProducts(){
      productsEl.innerHTML = '';
      PRODUCTS.forEach(p => {
        const card = document.createElement('article');
        card.className = 'card';
        card.innerHTML = `
          <img src="${p.img}" alt="${p.name}">
          <div class="card-body">
            <div style="display:flex; align-items:center; gap:0.5rem">
              <div style="font-weight:700">${p.name}</div>
              <div class="price">${p.price}</div>
            </div>
            <div class="small" style="margin-top:0.5rem">${p.desc}</div>
            <div class="actions">
              <button class="btn" data-id="${p.id}" style="background:var(--accent); color:white">Réserver</button>
              <button class="btn" data-id="${p.id}" style="background:transparent">Voir</button>
            </div>
          </div>
        `;
        productsEl.appendChild(card);
      });
    }

    // Modal logic
    const overlay = document.getElementById('overlay');
    const adminOverlay = document.getElementById('admin-overlay');
    const modalTitle = document.getElementById('modal-title');
    const summaryImg = document.getElementById('summary-img');
    const summaryName = document.getElementById('summary-name');
    const summaryDesc = document.getElementById('summary-desc');
    const summaryPrice = document.getElementById('summary-price');
    const productIdInput = document.getElementById('product-id');

    function openReservation(productId){
      const p = PRODUCTS.find(x=>x.id===productId);
      if(!p) return;
      modalTitle.textContent = 'Réserver — ' + p.name;
      summaryImg.src = p.img;
      summaryImg.alt = p.name;
      summaryName.textContent = p.name;
      summaryDesc.textContent = p.desc;
      summaryPrice.textContent = p.price;
      productIdInput.value = p.id;
      overlay.style.display = 'flex';
      overlay.setAttribute('aria-hidden','false');
    }
    document.getElementById('close-modal').addEventListener('click', ()=>{ overlay.style.display='none'; overlay.setAttribute('aria-hidden','true'); });

    document.addEventListener('click', (e)=>{
      const btn = e.target.closest('button[data-id]');
      if(btn){
        const id = btn.getAttribute('data-id');
        if(btn.textContent.trim().toLowerCase()==='réserver') openReservation(id);
        else {
          const p = PRODUCTS.find(x=>x.id===id);
          alert(p.name + '\n\n' + p.desc + '\nPrix: ' + p.price);
        }
      }
    });

    // Reservation submit
    document.getElementById('reservation-form').addEventListener('submit', (ev)=>{
      ev.preventDefault();
      const id = productIdInput.value;
      const name = document.getElementById('res-name').value.trim();
      const phone = document.getElementById('res-phone').value.trim();
      const email = document.getElementById('res-email').value.trim();
      const date = document.getElementById('res-date').value;
      const slot = document.getElementById('res-slot').value;
      const qty = parseInt(document.getElementById('res-qty').value,10) || 1;
      const note = document.getElementById('res-note').value.trim();

      if(!name || !phone || !email || !date) { alert('Remplis tous les champs requis.'); return; }

      // Basic availability check: same product, same date & slot
      const reservations = loadReservations();
      const conflict = reservations.find(r=> r.productId===id && r.date===date && r.slot===slot);
      if(conflict){
        if(!confirm('Il existe déjà une réservation pour ce créneau. Continuer quand même?')) return;
      }

      const r = { id: 'r_' + Date.now(), productId: id, name, phone, email, date, slot, qty, note, createdAt: new Date().toISOString() };
      reservations.push(r);
      saveReservations(reservations);

      document.getElementById('reservation-result').style.display='block';
      document.getElementById('reservation-result').innerHTML = `<div style="padding:0.75rem; background:linear-gradient(90deg, rgba(139,94,60,0.1), rgba(206,154,107,0.08)); border-radius:8px">Réservation enregistrée ✅<br><small class='small'>Nous te contacterons à ${email} pour confirmer.</small></div>`;

      // reset form after short delay
      setTimeout(()=>{
        overlay.style.display='none'; overlay.setAttribute('aria-hidden','true');
        document.getElementById('reservation-form').reset();
        document.getElementById('reservation-result').style.display='none';
      }, 1400);
    });

    // Contact form (demo: enregistrer localement)
    document.getElementById('contact-form').addEventListener('submit', (ev)=>{
      ev.preventDefault();
      alert('Merci! Message reçu (démo).');
      ev.target.reset();
    });

    // Admin
    document.getElementById('open-admin').addEventListener('click', (e)=>{ e.preventDefault(); adminOverlay.style.display='flex'; adminOverlay.setAttribute('aria-hidden','false'); });
    document.getElementById('close-admin').addEventListener('click', ()=>{ adminOverlay.style.display='none'; adminOverlay.setAttribute('aria-hidden','true'); });
    document.getElementById('admin-login').addEventListener('click', ()=>{
      const pass = document.getElementById('admin-pass').value;
      if(pass === 'admin123'){
        document.getElementById('admin-area').style.display='block';
        renderAdminList();
      } else alert('Mot de passe incorrect');
    });

    function renderAdminList(){
      const list = document.getElementById('admin-list');
      const res = loadReservations();
      if(res.length===0){ list.innerHTML = '<div class="small">Aucune réservation.</div>'; return; }
      list.innerHTML = '';
      res.slice().reverse().forEach(r=>{
        const p = PRODUCTS.find(x=>x.id===r.productId) || {};
        const el = document.createElement('div');
        el.style.padding='0.5rem'; el.style.borderBottom='1px solid #eee';
        el.innerHTML = `<div style="font-weight:700">${p.name || r.productId} — ${r.date} (${r.slot})</div>
                        <div class="small">${r.name} · ${r.email} · ${r.phone} · qté: ${r.qty}</div>
                        <div class="small">${r.note || ''}</div>
                        <div class="small">Réservé: ${new Date(r.createdAt).toLocaleString()}</div>`;
        list.appendChild(el);
      });
    }

    // Export CSV for admin
    document.getElementById('export-csv').addEventListener('click', ()=>{
      const reservations = loadReservations();
      if(reservations.length===0){ alert('Aucune réservation à exporter.'); return; }
      const header = ['id','productId','productName','name','email','phone','date','slot','qty','note','createdAt'];
      const rows = reservations.map(r=> {
        const p = PRODUCTS.find(x=>x.id===r.productId) || {name:''};
        return [r.id, r.productId, p.name, r.name, r.email, r.phone, r.date, r.slot, r.qty, (r.note||'').replace(/\n/g,' '), r.createdAt];
      });
      const csv = [header, ...rows].map(r=> r.map(cell => '"' + String(cell).replace(/"/g,'""') + '"').join(',')).join('\n');
      const blob = new Blob([csv], {type: 'text/csv;charset=utf-8;'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a'); a.href = url; a.download = 'reservations.csv'; a.click(); URL.revokeObjectURL(url);
    });

    // Misc
    document.getElementById('scroll-catalog').addEventListener('click', ()=>{ document.getElementById('catalog').scrollIntoView({behavior:'smooth'}); });
    document.getElementById('year').textContent = new Date().getFullYear();

    // Render on load
    renderProducts();

    // Accessibility: close overlay on Escape
    document.addEventListener('keydown', (e)=>{
      if(e.key === 'Escape'){
        overlay.style.display='none'; overlay.setAttribute('aria-hidden','true');
        adminOverlay.style.display='none'; adminOverlay.setAttribute('aria-hidden','true');
      }
    });
  </script></body>
</html>
