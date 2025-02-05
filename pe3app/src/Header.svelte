<script>
    export let navigateTo; // Funktion zum Navigieren
</script>

<header>
    <h1>
        <button class="home-button" on:click={() => navigateTo('/')}>Wilde Tiere</button>
    </h1>
    <div class="buttons">
        <button on:click={() => navigateTo('generator')}>Generator</button>
        <button on:click={() => navigateTo('recents')}>Recents</button>
    </div>
</header>

<style>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background-color: #ffffff;
        color: black;
        padding: 1rem;
    }

    h1 {
        margin: 0;
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
    }

    button:hover {
        background-color: #000000;
        color: rgb(255, 252, 252);
    }
</style>
