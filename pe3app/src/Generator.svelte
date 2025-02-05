<script>
    // =========================
    // Exportierte Props aus Parent
    // =========================
    export let imagePrompt = '';
    export let isLoading = false;
    export let generatedImage = null;
    export let runPrompt; // <- Wird vom Parent bereitgestellt

    // =========================
    // Slider-State
    // =========================
    let cuteness = 50;
    let aggression = 50;
    let sadness = 50;
    let fluffness = 50;
    let scales = 50;
    let feathers = 50;
    let size = 50;

    // =========================
    // Tier-Datenbank
    // =========================
    const animalCatalog = [
        { name: "Bunny", attributes: ["cute", "small", "fluffy"] },
        { name: "Kitten", attributes: ["cute", "small", "fluffy"] },
        { name: "Puppy", attributes: ["cute", "small", "fluffy"] },
        { name: "Hamster", attributes: ["cute", "small", "fluffy"] },
        { name: "Fennec Fox", attributes: ["cute", "small", "fluffy"] },
        { name: "Tiger", attributes: ["aggressive", "large"] },
        { name: "Lion", attributes: ["aggressive", "large"] },
        { name: "Wolverine", attributes: ["aggressive", "small"] },
        { name: "T-Rex", attributes: ["aggressive", "large", "scales"] },
        { name: "Shark", attributes: ["aggressive", "scales"] },
        { name: "Crocodile", attributes: ["aggressive", "large", "scales"] },
        { name: "Dragon", attributes: ["aggressive", "scales"] },
        { name: "Snake", attributes: ["aggressive", "scales", "small"] },
        { name: "Penguin", attributes: ["sad"] },
        { name: "Eeyore", attributes: ["sad", "fluffy"] },
        { name: "Elephant", attributes: ["sad", "large"] },
        { name: "Sloth", attributes: ["sad", "small", "fluffy"] },
        { name: "Panda", attributes: ["fluffy", "large", "cute"] },
        { name: "Alpaca", attributes: ["fluffy", "cute"] },
        { name: "Unicorn", attributes: ["fluffy", "cute"] },
        { name: "Red Panda", attributes: ["fluffy", "cute", "small"] },
        { name: "Lizard", attributes: ["scales", "small"] },
        { name: "Phoenix", attributes: ["feathers"] },
        { name: "Peacock", attributes: ["feathers", "cute"] },
        { name: "Griffin", attributes: ["feathers", "aggressive", "large"] },
        { name: "Eagle", attributes: ["feathers", "aggressive"] },
        { name: "Ostrich", attributes: ["feathers", "large"] },
        { name: "Giraffe", attributes: ["large"] },
        { name: "Whale", attributes: ["large"] },
        { name: "Hippopotamus", attributes: ["large", "aggressive"] },
        { name: "Rhino", attributes: ["large", "aggressive"] },
        { name: "Polar Bear", attributes: ["aggressive", "large", "fluffy"] },
        { name: "Grizzly Bear", attributes: ["aggressive", "large", "fluffy"] },
        { name: "Wolf", attributes: ["aggressive", "fluffy"] },
        { name: "Koala", attributes: ["cute", "small", "fluffy"] },
        { name: "Bat", attributes: ["small", "aggressive"] },
        { name: "Hummingbird", attributes: ["feathers", "small", "cute"] },
        { name: "Toucan", attributes: ["feathers", "cute", "small"] },
        { name: "Flamingo", attributes: ["feathers", "large", "cute"] },
        { name: "Chinchilla", attributes: ["cute", "small", "fluffy"] },
        { name: "Polar Fox", attributes: ["aggressive", "cute", "small", "fluffy"] },
        { name: "Chimera", attributes: ["aggressive", "large", "scales", "fluffy"] },
        { name: "Hydra", attributes: ["aggressive", "large", "scales"] },
        { name: "Yeti", attributes: ["aggressive", "large", "fluffy"] },
        { name: "Cerberus", attributes: ["aggressive", "large", "fluffy"] },
        { name: "Harpy", attributes: ["aggressive", "feathers", "small"] },
        { name: "Hippogriff", attributes: ["feathers", "large", "fluffy", "aggressive"] },
        { name: "Pegasus", attributes: ["feathers", "large", "fluffy"] },
        { name: "Dragon Pup", attributes: ["aggressive", "small", "scales", "cute"] },
       
    ];

    // Tier, das letztendlich ausgewählt wurde
    let currentAnimal = "Bunny";
    let generatedPrompt = '';

    // (A) Sliderwerte in passende Attribute umwandeln
    function getUserAttributes() {
        const attrs = [];
        if (cuteness > 70) attrs.push("cute");
        if (aggression > 70) attrs.push("aggressive");
        if (sadness > 70) attrs.push("sad");
        if (fluffness > 70) attrs.push("fluffy");
        if (scales > 70) attrs.push("scales");
        if (feathers > 70) attrs.push("feathers");
        if (size > 70) {
            attrs.push("large");
        } else if (size < 30) {
            attrs.push("small");
        }
        return attrs;
    }

    // (B) Bestpassendes Tier auswählen
    function selectBestAnimal() {
        const wantedAttrs = getUserAttributes();
        let bestMatchCount = 0;
        let bestAnimals = [];

        for (const candidate of animalCatalog) {
            let matchCount = 0;
            for (const attr of wantedAttrs) {
                if (candidate.attributes.includes(attr)) {
                    matchCount++;
                }
            }
            if (matchCount > bestMatchCount) {
                bestMatchCount = matchCount;
                bestAnimals = [candidate.name];
            } else if (matchCount === bestMatchCount) {
                bestAnimals.push(candidate.name);
            }
        }
        // Fallback: falls gar keine Treffer
        if (bestAnimals.length === 0) {
            return "Bunny";
        }
        // Zufällige Auswahl unter den besten
        const randomIndex = Math.floor(Math.random() * bestAnimals.length);
        return bestAnimals[randomIndex];
    }

    // (C) Beschreibungs-Strings
    function generateDescription() {
        const descriptions = [];
        if (cuteness < 30) {
            descriptions.push("the animal should look threatening");
        } else if (cuteness > 70) {
            descriptions.push("the animal should look very cute");
        }
        if (aggression < 30) {
            descriptions.push("the animal should look friendly");
        } else if (aggression > 70) {
            descriptions.push("the animal should look very aggressive");
        }
        if (sadness < 30) {
            descriptions.push("the animal should look happy");
        } else if (sadness > 70) {
            descriptions.push("the animal should look sad");
        }
        if (fluffness < 30) {
            descriptions.push("the animal should have short hair");
        } else if (fluffness > 70) {
            descriptions.push("the animal should be very fluffy");
        }
        if (scales > 70) {
            descriptions.push("the animal should have many scales");
        }
        if (feathers > 70) {
            descriptions.push("the animal should have many feathers");
        }
        if (size < 30) {
            descriptions.push("the animal should look small");
        } else if (size > 70) {
            descriptions.push("the animal should look very large");
        }

        return descriptions.join(", ");
    }

    // (D) Prompt zusammenbauen
    function generateDynamicPrompt() {
        const description = generateDescription();
        return `${imagePrompt.trim()}, animal reference: ${currentAnimal}${ description ? ", " + description : ""}`;
    }

    // (E) Button-Klick -> Tier bestimmen + Prompt generieren
    function executePrompt() {
        currentAnimal = selectBestAnimal();
        const dynamicPrompt = generateDynamicPrompt();
        generatedPrompt = dynamicPrompt;

        // WICHTIG: Jetzt zusätzlich die Sliderwerte mit übergeben!
        runPrompt(dynamicPrompt, {
            cuteness,
            aggression,
            sadness,
            fluffness,
            scales,
            feathers,
            size
        });
    }
</script>

<section class="generator-section">
    <div class="layout">
        <!-- Linke Spalte: Bild-Vorschau -->
        <div class="image-preview">
            {#if generatedImage}
                <img src={generatedImage} alt="Generated Animal Image"/>
               
            {:else}
                <div class="placeholder">
                    <p>No image generated yet</p>
                </div>
            {/if}
        </div>

        <!-- Rechte Spalte: Slider -->
        <div class="sliders">
            <div class="slider-group">
                <label for="cuteness-slider">Cuteness</label>
                <input id="cuteness-slider" type="range" min="0" max="100" bind:value={cuteness} />
            </div>
            <div class="slider-group">
                <label for="aggression-slider">Aggression</label>
                <input id="aggression-slider" type="range" min="0" max="100" bind:value={aggression} />
            </div>
            <div class="slider-group">
                <label for="sadness-slider">Sadness</label>
                <input id="sadness-slider" type="range" min="0" max="100" bind:value={sadness} />
            </div>
            <div class="slider-group">
                <label for="fluffness-slider">Fluffness</label>
                <input id="fluffness-slider" type="range" min="0" max="100" bind:value={fluffness} />
            </div>
            <div class="slider-group">
                <label for="scales-slider">Scales</label>
                <input id="scales-slider" type="range" min="0" max="100" bind:value={scales} />
            </div>
            <div class="slider-group">
                <label for="feathers-slider">Feathers</label>
                <input id="feathers-slider" type="range" min="0" max="100" bind:value={feathers} />
            </div>
            <div class="slider-group">
                <label for="size-slider">Size</label>
                <input id="size-slider" type="range" min="0" max="100" bind:value={size} />
            </div>
        </div>
    </div>

    <!-- Button -->
    <div class="button-container">
        <button class="generate-button" on:click={executePrompt} disabled={isLoading}>
            {isLoading ? 'Loading...' : 'Generate'}
        </button>
    </div>
</section>

<style>
    /* Zentriert alles auf dem Bildschirm */
    .generator-section {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        min-height: 80vh; /* Füllt den ganzen Viewport in der Höhe */
        font-family: Arial, sans-serif;
        gap: 2rem;
        padding: 2rem;
        box-sizing: border-box;
    }

    .layout {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        gap: 2rem;
        width: 100%;
        max-width: 1200px; /* oder ein Wert nach Geschmack */
    }

    .image-preview {
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .image-preview img {
        max-width: 100%;
        max-height: 500px;
        border-radius: 8px;
    }

    .placeholder {
        border: 2px dashed #ccc;
        width: 300px;
        height: 300px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 8px;
    }

    .generated-prompt {
        margin-top: 1rem;
        font-size: 0.9rem;
        color: #555;
        text-align: center;
    }

    .sliders {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }

    .slider-group {
        display: flex;
        flex-direction: column;
    }

    .slider-group label {
        margin-bottom: 0.5rem;
        font-weight: bold;
    }

    .button-container {
        text-align: center;
    }

    .generate-button {
        padding: 1rem 2rem;
        font-size: 1rem;
        border-radius: 4px;
        border: 1px solid #ffffff;
        background-color: #fff;
        cursor: pointer;
    }
    button:hover {
        background-color: #000000;
        color: rgb(255, 252, 252);
    }
</style>
