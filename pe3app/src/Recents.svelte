<script>
    // Wir übernehmen das Sortiersystem UND die Attributdarstellung

    export let recents = [];

    // Sortier-Logik
    let sortAttribute = "cuteness";
    let sortDirection = "desc"; // "asc" oder "desc"

    function sortRecents() {
        recents = recents.slice().sort((a, b) => {
            let compare = (b[sortAttribute] ?? 0) - (a[sortAttribute] ?? 0);
            return sortDirection === "desc" ? compare : compare * -1;
        });
    }

    // Welche Attribute wir ausgeben wollen
    const attributeKeys = [
        "sadness",
        "aggression",
        "cuteness",
        "fluffness",
        "scales",
        "feathers",
        "size"
    ];
</script>

<!-- Sortier-Auswahl und Button -->
<div class="sort-controls">
    <label>Sort by:</label>
    <select bind:value={sortAttribute} on:change={sortRecents}>
        <option value="cuteness">Cuteness</option>
        <option value="aggression">Aggression</option>
        <option value="sadness">Sadness</option>
        <option value="fluffness">Fluffness</option>
        <option value="scales">Scales</option>
        <option value="feathers">Feathers</option>
        <option value="size">Size</option>
    </select>

    <button on:click={() => {
        sortDirection = sortDirection === 'desc' ? 'asc' : 'desc';
        sortRecents();
    }}>
        Sort direction: {sortDirection === 'desc' ? 'Descending' : 'Ascending'}
    </button>
</div>

<!-- Grid mit den Tieren -->
<div class="recents-container">
    {#each recents as recent (recent.imageUrl)}
        <div class="recent-item">
            <img src={recent.imageUrl} alt="Recent" />
            <h3 class="animal-name">
                {recent.prompt
                    ?.split(':')?.[1]
                    ?.split(',')?.[0]
                    ?.trim() || 'Unbekanntes Tier'}
            </h3>
            <!-- Ausgabe aller relevanten Attribute untereinander -->
            {#each attributeKeys as key}
                {#if recent[key] !== undefined && recent[key] !== null}
                    <p class="attribute-line">{key}: {recent[key]}</p>
                {/if}
            {/each}
        </div>
    {/each}
</div>

<style>
/* Global Styles */
body {
    background-color: #ffffff;
    color: black;
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
}

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #ffffff;
    color: black;
    padding: 1rem;
    border-bottom: 1px solid #ccc;
}

h1 {
    margin: 0;
    font-size: 1.5rem;
    font-weight: bold;
}

.buttons {
    display: flex;
    gap: 1rem;
}

button {
    background-color: #ffffff;
    color: black;
    border: 1px solid #ffffff;
    padding: 0.5rem 1rem;
    cursor: pointer;
    font-size: 1rem;
    border-radius: 4px;
    transition: background-color 0.2s, color 0.2s;
}

button:hover {
    background-color: #000000;
    color: rgb(255, 255, 255);
}

button:active {
    background-color: #ffffff;
}

.home-button {
    background: none;
    color: black;
    border: none;
    font-size: 1.5rem;
    font-weight: bold;
    cursor: pointer;
    padding: 0;
}

.home-button:hover {
    text-decoration: underline;
}

/* Main Styles */
main {
    padding: 2rem;
}

.image-generator {
    margin-top: 1rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

.image-generator input {
    padding: 0.5rem;
    font-size: 1rem;
    border-radius: 4px;
    border: 1px solid #ffffff;
    color: black;
    background-color: #ffffff;
}

.generated-image img {
    width: 512px; /* Hier kannst du gerne noch größer werden */
    height: 512px;
    border-radius: 8px;
    border: 1px solid #ffffff;
}

/* ======= Bereinigte Recents Section ======= */
.recents-container {
    display: grid;
    /* Hier für größere Kacheln min. 300px oder 350px wählen */
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 1rem;
    
    overflow-y: auto; /* Scrollen bei Überlauf */
    padding: 1rem;
}

.recent-item {
    text-align: center;
    background: #ffffff;
    border: 1px solid #ffffff;
    border-radius: 8px;
    padding: 1rem;
}

.recent-item img {
    width: 100%;
    height: auto; /* <-- Statt 100% */
    object-fit: contain;
    border-radius: 8px;
    margin-bottom: 1rem;
}


/* Nur den Anfangsbuchstaben groß */
.attribute-line {
    margin: 0.25rem 0;
    color: #333;
    font-size: 1rem;
    text-align: center;
    width: 100%;
    text-transform: capitalize; /* Macht nur den ersten Buchstaben groß */
}

@media (min-width: 1024px) {
    .recents-container {
        grid-template-columns: repeat(3, 1fr);
    }
}

@media (min-width: 768px) and (max-width: 1023px) {
    .recents-container {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 767px) {
    .recents-container {
        grid-template-columns: 1fr;
    }
}

.animal-name {
    font-size: 1.2rem;
    font-weight: bold;
    margin: 0.5rem 0;
    color: #333;
}
</style>
