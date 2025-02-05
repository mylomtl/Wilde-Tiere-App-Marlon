<!-- App.svelte -->

<script>
    import Header from './Header.svelte';
    import Generator from './Generator.svelte';
    import Recents from './Recents.svelte';

    let currentRoute = '/';
    let imagePrompt = '';
    let generatedImage = null;
    let includeImage = false;
    let recents = [];
    let isLoading = false;

    const API_KEY = import.meta.env.VITE_OPENAI_API_KEY || 'FALLBACK_API_KEY';

    const navigateTo = (route) => {
        currentRoute = route;
    };

    // ANPASSUNG: runPrompt nimmt jetzt ZWEI Parameter entgegen
    const runPrompt = async (dynamicPrompt, sliderData) => {
        if (!dynamicPrompt.trim()) {
            alert('Bitte geben Sie einen Prompt ein!');
            return;
        }

        isLoading = true;
        try {
            const imageResponse = await fetch('https://api.openai.com/v1/images/generations', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    Authorization: `Bearer ${API_KEY}`,
                },
                body: JSON.stringify({
                    prompt: dynamicPrompt,
                    n: 1,
                    size: '512x512',
                }),
            });

            if (!imageResponse.ok) {
                throw new Error(`Fehler: ${imageResponse.statusText}`);
            }

            const imageData = await imageResponse.json();

            if (!imageData.data || imageData.data.length === 0) {
                throw new Error('Die API-Antwort enthält keine Bilddaten.');
            }

            generatedImage = imageData.data[0].url;

            // Hier schreiben wir ALLES in recents: Prompt, Bild-URL und Sliderwerte
            recents = [
                {
                    prompt: dynamicPrompt,
                    imageUrl: generatedImage,
                    ...sliderData // Spread-Operator fügt alle { cuteness, aggression, ... } ein
                },
                ...recents
            ];
        } catch (error) {
            alert(`Ein Fehler ist aufgetreten: ${error.message}`);
        } finally {
            isLoading = false;
        }
    };
</script>

<Header {navigateTo} />

<main>
    {#if currentRoute === '/'}
    <div class="welcome-text">
        <h1>Erstelle dein eigenes Tier!</h1>
        <p>
            In dieser Website könnt ihr durch Slider Tiere kreieren, die am meisten zu den 
            Eigenschaften passen, die ihr eingegeben habt. Ob niedlich, aggressiv oder besonders 
            flauschig - ihr bestimmt, wie euer Tier aussehen soll!
        </p>
        <p>
            Im Recents-Bereich könnt ihr alle eure erstellten Tiere betrachten und miteinander 
            vergleichen. Sortiert sie nach verschiedenen Eigenschaften und entdeckt, welche 
            Kombinationen die interessantesten Ergebnisse liefern.
        </p>
        <p>
           
        </p>
    </div>
    {:else if currentRoute === 'generator'}
        <!-- Generator ruft runPrompt(dynamicPrompt, sliderData) auf -->
        <Generator
            bind:imagePrompt
            bind:includeImage
            bind:isLoading
            {generatedImage}
            {runPrompt}
        />
    {:else if currentRoute === 'recents'}
        <Recents {recents} />
    {/if}
</main>

<style>
    .welcome-text {
        max-width: 800px;
        margin: 2rem auto;
        padding: 0 1rem;
        text-align: center;
        color: #000000;
    }

    .welcome-text h1 {
        font-size: 2.5rem;
        margin-bottom: 1.5rem;
        color: #000000;
    }

    .welcome-text p {
        font-size: 1rem;
        line-height: 1.6;
        margin-bottom: 1rem;
    }

    .start-section {
        /* So stellst du die Breite ein */
        max-width: 600px; 
        /* Zentriert den Abschnitt horizontal */
        margin: 0 auto;   
        /* Optional: Abstände oben/unten */
        padding: 1rem;    
    }

    .limit-text {
        /* Wenn du die Zeilenlänge begrenzen möchtest */
        line-height: 1.5;
    }
    main {
        padding: 2rem;
    }
</style>
