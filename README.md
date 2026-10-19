# bacheca_itissgm
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bacheca Segnalazioni - ITIS San Gregorio Magno</title>
  <style>
    :root {
      --primary: #0056b3;
      --bg: #f4f7f6;
      --card-bg: #ffffff;
      --text: #333;
    }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--bg);
      color: var(--text);
      margin: 0;
      padding: 20px;
    }
    .container {
      max-width: 800px;
      margin: 0 auto;
    }
    header {
      text-align: center;
      margin-bottom: 30px;
    }
    h1 { color: var(--primary); margin-bottom: 5px; }
    .card {
      background: var(--card-bg);
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.05);
      margin-bottom: 20px;
    }
    .form-group {
      margin-bottom: 15px;
    }
    label { display: block; margin-bottom: 5px; font-weight: bold; }
    input[type="text"], textarea, select {
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 6px;
      box-sizing: border-box;
    }
    .checkbox-group {
      display: flex;
      align-items: center;
      gap: 10px;
    }
    button {
      background-color: var(--primary);
      color: white;
      border: none;
      padding: 12px 20px;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
      width: 100%;
    }
    button:hover { opacity: 0.9; }
    .post {
      border-left: 5px solid var(--primary);
      padding: 15px;
      margin-bottom: 15px;
      background: #fff;
      border-radius: 0 8px 8px 0;
      box-shadow: 0 2px 5px rgba(0,0,0,0.05);
    }
    .post-header {
      display: flex;
      justify-content: space-between;
      font-size: 0.85em;
      color: #666;
      margin-bottom: 8px;
    }
    .badge-anon { color: #d9534f; font-weight: bold; }
    .admin-actions {
      margin-top: 10px;
      padding-top: 10px;
      border-top: 1px dashed #ccc;
      display: flex;
      gap: 10px;
    }
    .btn-copy { background-color: #28a745; }
    .btn-delete { background-color: #dc3545; }
  </style>
</head>
<body>

<div class="container">
  <header>
    <h1>ITIS San Gregorio Magno</h1>
    <p>Bacheca Digitale Segnalazioni e Problematiche</p>
  </header>

  <!-- FORM INVIO SEGNALAZIONE -->
  <div class="card">
    <h2>Invia una Segnalazione</h2>
    <form id="issueForm">
      <div class="form-group">
        <label for="title">Oggetto della problematica</label>
        <input type="text" id="title" placeholder="Es. Riscaldamenti aula 3B non funzionanti" required>
      </div>

      <div class="form-group">
        <label for="category">Categoria</label>
        <select id="category">
          <option value="Struttura/Aule">Struttura / Aule</option>
          <option value="Laboratori/Computer">Laboratori / PC</option>
          <option value="Didattica/Orari">Didattica / Orari</option>
          <option value="Altro">Altro</option>
        </select>
      </div>

      <div class="form-group">
        <label for="description">Descrizione dettagliata</label>
        <textarea id="description" rows="4" placeholder="Spiega il problema nei dettagli..." required></textarea>
      </div>

      <div class="form-group checkbox-group">
        <input type="checkbox" id="anonymous" onchange="toggleAuthorInput(this)">
        <label for="anonymous" style="margin:0;">Invia in modo anonimo</label>
      </div>

      <div class="form-group" id="authorGroup">
        <label for="author">Il tuo Nome e Classe</label>
        <input type="text" id="author" placeholder="Es. Mario Rossi - 4A">
      </div>

      <button type="submit">Invia Segnalazione</button>
    </form>
  </div>

  <!-- LISTA SEGNALAZIONI -->
  <h2>Bacheca Segnalazioni</h2>
  <div id="postsContainer">
    <!-- I post verranno caricati qui -->
  </div>
</div>

<script>
  // Gestione dinamica input autore/anonimo
  function toggleAuthorInput(checkbox) {
    const authorGroup = document.getElementById('authorGroup');
    if (checkbox.checked) {
      authorGroup.style.display = 'none';
      document.getElementById('author').value = '';
    } else {
      authorGroup.style.display = 'block';
    }
  }

  // Gestione invio form (Esempio Locale / Mock Backend)
  document.getElementById('issueForm').addEventListener('submit', function(e) {
    e.preventDefault();
    
    const title = document.getElementById('title').value;
    const category = document.getElementById('category').value;
    const description = document.getElementById('description').value;
    const isAnonymous = document.getElementById('anonymous').checked;
    const author = isAnonymous ? 'Anonimo' : (document.getElementById('author').value || 'Studente');

    const newPost = {
      id: Date.now(),
      title,
      category,
      description,
      author,
      date: new Date().toLocaleDateString('it-IT')
    };

    savePost(newPost);
    this.reset();
    document.getElementById('authorGroup').style.display = 'block';
    alert('Segnalazione inviata con successo!');
  });

  function savePost(post) {
    let posts = JSON.parse(localStorage.getItem('itis_posts')) || [];
    posts.unshift(post);
    localStorage.setItem('itis_posts', JSON.stringify(posts));
    renderPosts();
  }

  function renderPosts() {
    const container = document.getElementById('postsContainer');
    const posts = JSON.parse(localStorage.getItem('itis_posts')) || [];
    
    if(posts.length === 0) {
      container.innerHTML = '<p>Nessuna segnalazione presente.</p>';
      return;
    }

    container.innerHTML = posts.map(post => `
      <div class="post">
        <div class="post-header">
          <span><strong>${post.category}</strong> • ${post.date}</span>
          <span class="${post.author === 'Anonimo' ? 'badge-anon' : ''}">${post.author}</span>
        </div>
        <h3 style="margin: 5px 0;">${post.title}</h3>
        <p style="margin: 5px 0;">${post.description}</p>
        
        <!-- PULSANTE ADMIN PER FORMATTARE SU INSTAGRAM -->
        <div class="admin-actions">
          <button class="btn-copy" onclick="copyForInstagram('${encodeURIComponent(JSON.stringify(post))}')">
            📋 Copia testo per Instagram
          </button>
        </div>
      </div>
    `).join('');
  }

  // Funzione per formattare il testo ed incollarlo su IG
  function copyForInstagram(encodedPost) {
    const post = JSON.parse(decodeURIComponent(encodedPost));
    const text = `⚠️ SEGNALAZIONE ITIS ⚠️\n\n📌 Categoria: ${post.category}\n❓ Problema: ${post.title}\n💬 Dettagli: ${post.description}\n👤 Inviato da: ${post.author}\n\n#ITIS #SanGregorioMagno #Segnalazioni`;
    
    navigator.clipboard.writeText(text).then(() => {
      alert('Testo copiato!puoi incollarlo su Instagram o inviarlo tramite app.');
    });
  }

  // Caricamento iniziale
  renderPosts();
</script>

</body>
</html>
