---
layout: home
title: Główny Panel
---

### 🚀 Dane z domowego serwera Python

<div id="status-container" style="background: #e1f5fe; border-left: 5px solid #03a9f4; padding: 15px; margin: 20px 0; border-radius: 4px;">
    <p><strong>Status:</strong> Oczekiwanie na dane...</p>
    <p><strong>Wartość:</strong> --</p>
</div>

<p><small>Ostatnia aktualizacja: <span id="czas-aktualizacji">brak</span></small></p>

<script>
function pobierzDane() {
    // Pobieramy plik JSON z folderu assets z mechanizmem unikania cache (?t=...)
    fetch('assets/dane.json?t=' + new Date().getTime())
        .then(response => {
            if (!response.ok) throw new Error('Brak pliku danych na serwerze.');
            return response.json();
        })
        .then(data => {
            document.getElementById('status-container').innerHTML = `
                <p><strong>Status:</strong> ${data.status}</p>
                <p><strong>Wartość z Pythona:</strong> ${data.wartosc}</p>
            `;
            document.getElementById('czas-aktualizacji').innerText = new Date().toLocaleTimeString();
        })
        .catch(error => {
            document.getElementById('status-container').innerHTML = `<p style="color:red;"><strong>Błąd:</strong> ${error.message}</p>`;
        });
}

// Uruchom od razu i odświeżaj w przeglądarce co 5 minut
pobierzDane();
setInterval(pobierzDane, 300000);
</script>
