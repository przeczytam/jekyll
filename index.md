---
layout: page
title: 📊 Panel Zarządzania Domem
---

<style>
/* Stylizacja nowoczesnych zakładek */
.tabs-nav {
    display: flex;
    gap: 10px;
    border-bottom: 2px solid #e1e4e8;
    padding-bottom: 0;
    margin-bottom: 25px;
}
.tab-btn {
    background: none;
    border: none;
    padding: 10px 20px;
    font-size: 16px;
    font-weight: 500;
    color: #586069;
    cursor: pointer;
    border-bottom: 2px solid transparent;
    margin-bottom: -2px;
    transition: all 0.2s ease;
}
.tab-btn:hover {
    color: #0366d6;
}
.tab-btn.active {
    color: #0366d6;
    border-bottom: 2px solid #0366d6;
    font-weight: 600;
}
.tab-content {
    display: none;
    animation: fadeIn 0.3s ease;
}
.tab-content.active {
    display: block;
}

/* Stylizacja kart z danymi */
.grid-cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
    margin-top: 15px;
}
.data-card {
    background: #ffffff;
    border: 1px solid #e1e4e8;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.02);
}
.data-card h4 { margin: 0 0 10px 0; color: #586069; font-size: 14px; text-transform: uppercase; }
.data-card .value { font-size: 28px; font-weight: bold; color: #24292e; margin: 0; }

@keyframes fadeIn {
    from { opacity: 0; transform: translateY(5px); }
    to { opacity: 1; transform: translateY(0); }
}
</style>

<!-- Nawigacja zakładek -->
<div class="tabs-nav">
    <button class="tab-btn active" onclick="switchTab(event, 'panel')">🏠 Podgląd</button>
    <button class="tab-btn" onclick="switchTab(event, 'statystyki')">📈 Statystyki</button>
    <button class="tab-btn" onclick="switchTab(event, 'system')">⚙️ Serwer</button>
</div>

<!-- ZAWARTOŚĆ ZAKŁADKI 1: PANEL GŁÓWNY -->
<div id="panel" class="tab-content active">
    <h3>Aktualny stan systemu</h3>
    <p>Oto najświeższe odczyty wygenerowane przez Twój skrypt Python:</p>
    
    <div class="grid-cards">
        <div class="data-card">
            <h4>Status urządzenia</h4>
            <p id="txt-status" class="value">--</p>
        </div>
        <div class="data-card">
            <h4>Główna wartość</h4>
            <p id="txt-wartosc" class="value">--</p>
        </div>
    </div>
</div>

<!-- ZAWARTOŚĆ ZAKŁADKI 2: STATYSTYKI -->
<div id="statystyki" class="tab-content">
    <h3>Wykresy i analiza danych</h3>
    <p>Miejsce na Twoje wykresy. Możesz tu w przyszłości podpiąć np. bibliotekę Chart.js.</p>
    <div class="data-card" style="text-align: center; color: #888; padding: 40px 20px;">
        📊 [Wizualizacja historyczna pojawi się, gdy skrypt zacznie zbierać więcej danych]
    </div>
</div>

<!-- ZAWARTOŚĆ ZAKŁADKI 3: INFORMACJE SYSTEMOWE -->
<div id="system" class="tab-content">
    <h3>Metadane połączenia</h3>
    <table style="width: 100%; margin-top: 15px;">
        <tr>
            <td><strong>Źródło danych:</strong></td>
            <td>Lokalny skrypt Python via GitHub API</td>
        </tr>
        <tr>
            <td><strong>Interwał odświeżania:</strong></td>
            <td>Co 5 minut (cron)</td>
        </tr>
        <tr>
            <td><strong>Ostatnia synchronizacja:</strong></td>
            <td><span id="txt-sync">brak</span></td>
        </tr>
    </table>
</div>

<script>
// 1. Logika przełączania zakładek
function switchTab(event, tabId) {
    // Ukryj całą zawartość
    const contents = document.querySelectorAll('.tab-content');
    contents.forEach(content => content.classList.remove('active'));

    // Dezaktywuj wszystkie przyciski
    const buttons = document.querySelectorAll('.tab-btn');
    buttons.forEach(btn => btn.classList.remove('active'));

    // Pokaż wybraną zakładkę i aktywuj przycisk
    document.getElementById(tabId).classList.add('active');
    event.currentTarget.classList.add('active');
}

// 2. Logika dynamicznego pobierania danych z pliku JSON
function pobierzDane() {
    fetch('assets/dane.json?t=' + new Date().getTime())
        .then(response => {
            if (!response.ok) throw new Error('Nie udało się wczytać pliku JSON.');
            return response.json();
        })
        .then(data => {
            // Rozdzielamy dane do odpowiednich pól na różnych zakładkach
            document.getElementById('txt-status').innerText = data.status || 'Brak danych';
            document.getElementById('txt-wartosc').innerText = data.wartosc !== undefined ? data.wartosc : '--';
            document.getElementById('txt-sync').innerText = new Date().toLocaleTimeString();
        })
        .catch(error => {
            console.error('Błąd:', error);
        });
}

// Uruchomienie skryptu na starcie strony
pobierzDane();
// Automatyczne odświeżanie w tle co 5 minut
setInterval(pobierzDane, 300000);
</script>
