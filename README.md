# prenotazionideda
Prenotazioni ufficio irpino

<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Prenotazione Postazioni Ufficio</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background-color: #f5f5f5;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 20px;
    }
    h1 {
        margin-bottom: 30px;
    }
    .office {
        display: flex;
        flex-direction: row;
        gap: 80px;
        flex-wrap: wrap;
        justify-content: center;
    }
    .desk {
        position: relative;
        background-color: #d1e8ff;
        border: 2px solid #333;
        border-radius: 8px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 10px;
    }
    .desk.large { width: 250px; height: 250px; }
    .desk.single { width: 150px; height: 120px; }

    .seat {
        width: 50px;
        height: 50px;
        background-color: #b0b0b0;
        border-radius: 50%;
        position: absolute;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: background-color 0.3s;
        cursor: pointer;
    }

    .seat input {
        width: 80px;
        font-size: 12px;
        text-align: center;
        background: transparent;
        border: none;
        outline: none;
        pointer-events: none; /* input solo modificabile cliccando sulla sedia */
    }

    /* Tooltip */
    .seat:hover::after {
        content: attr(data-tooltip);
        position: absolute;
        top: -30px;
        left: 50%;
        transform: translateX(-50%);
        background-color: #333;
        color: #fff;
        padding: 5px 10px;
        border-radius: 4px;
        white-space: nowrap;
        font-size: 12px;
        z-index: 10;
    }

    /* Posizioni sedie scrivania grande 2+2 */
    #desk1-seat { top: -25px; left: 40px; }
    #desk2-seat { top: -25px; right: 40px; }
    #desk3-seat { bottom: -25px; left: 40px; }
    #desk4-seat { bottom: -25px; right: 40px; }

    /* Sedia scrivania singola */
    #desk5-seat { top: -25px; left: 50%; transform: translateX(-50%); }

    button {
        margin-top: 30px;
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
    }
</style>
</head>
<body>

<h1>Prenotazione Postazioni Ufficio</h1>

<div class="office">

    <!-- Scrivania grande 2+2 -->
    <div class="desk large" id="desk1-div">
        <div class="seat" id="desk1-seat" data-tooltip="Libera">
            <input type="text" placeholder="Nome 1" id="desk1">
        </div>
        <div class="seat" id="desk2-seat" data-tooltip="Libera">
            <input type="text" placeholder="Nome 2" id="desk2">
        </div>
        <div class="seat" id="desk3-seat" data-tooltip="Libera">
            <input type="text" placeholder="Nome 3" id="desk3">
        </div>
        <div class="seat" id="desk4-seat" data-tooltip="Libera">
            <input type="text" placeholder="Nome 4" id="desk4">
        </div>
        <span style="position:absolute; bottom:10px;">Scrivania Grande</span>
    </div>

    <!-- Scrivania singola -->
    <div class="desk single" id="desk5-div">
        <div class="seat" id="desk5-seat" data-tooltip="Libera">
            <input type="text" placeholder="Nome 5" id="desk5">
        </div>
        <span style="position:absolute; bottom:10px;">Scrivania Singola</span>
    </div>

</div>

<button onclick="saveReservation()">Prenota</button>

<script>
// Funzione per aggiornare colori e tooltip delle sedie
function updateSeatStatus() {
    for (let i = 1; i <= 5; i++) {
        const seat = document.getElementById("desk" + i + "-seat");
        const input = document.getElementById("desk" + i);
        const name = input.value.trim();
        if (name) {
            seat.style.backgroundColor = "#90ee90"; // verde chiaro
            seat.setAttribute("data-tooltip", "Prenotata da: " + name);
        } else {
            seat.style.backgroundColor = "#b0b0b0"; // grigio
            seat.setAttribute("data-tooltip", "Libera");
        }
    }
}

// Carica dati salvati all'apertura
window.onload = function() {
    for (let i = 1; i <= 5; i++) {
        const stored = localStorage.getItem("desk" + i);
        if (stored) {
            document.getElementById("desk" + i).value = stored;
        }
        updateSeatStatus();
        // Aggiorna in tempo reale
        document.getElementById("desk" + i).addEventListener("input", updateSeatStatus);
    }
};

// Salva prenotazioni nel localStorage
function saveReservation() {
    for (let i = 1; i <= 5; i++) {
        const value = document.getElementById("desk" + i).value.trim();
        if (value) {
            localStorage.setItem("desk" + i, value);
        } else {
            localStorage.removeItem("desk" + i);
        }
    }
    updateSeatStatus();
    alert("Prenotazioni aggiornate!");
}
</script>

</body>
</html>

