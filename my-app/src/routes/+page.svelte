
<script>
    let tekst = $state("");
    let nokkel = $state(0);
    let resultat = $state("");
    let feilmelding = $state("");

    let tekst2 = $state("");
    let nokkel2 = $state("");
    let resultat2 = $state("");
    let feilmelding2 = $state("");

    let tekst3 = $state("");
    let nokkel3 = $state("");
    let resultat3 = $state("");
    let feilmelding3 = $state("");

    const ALFABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

    // CAESAR CIPHER
    function kjorCaesar(retning) {
        feilmelding = "";
        resultat = "";

        if (!tekst.trim())
            return feilmelding = "Tekstfeltet kan ikke være tomt.";

        if (nokkel === null || nokkel < 0 || nokkel > 25 ||
            !Number.isInteger(Number(nokkel)))
            return feilmelding = "Nøkkel må være et heltall fra 0 til 25.";

        let midlertidigResultat = "";

        for (let i = 0; i < tekst.length; i++) {
            const b = tekst[i];

            if (b === " ") {
                midlertidigResultat += " ";
                continue;
            }

            const u = b.toUpperCase();
            const pos = ALFABET.indexOf(u);

            if (pos === -1)
                return feilmelding =
                    "Kun engelske bokstaver og mellomrom er tillatt.";

            let nyPos = (pos + Number(nokkel) * retning) % 26;
            if (nyPos < 0) nyPos += 26;

            midlertidigResultat +=
                b === u
                    ? ALFABET[nyPos]
                    : ALFABET[nyPos].toLowerCase();
        }

        resultat = midlertidigResultat;
    }

    // VIGENERE
    function kjorvigenere(retning) {
        feilmelding2 = "";
        resultat2 = "";

        if (!tekst2.trim())
            return feilmelding2 = "Tekstfeltet kan ikke være tomt.";

        const nokkel = nokkel2.toUpperCase();

        if (!/^[A-Z]+$/.test(nokkel))
            return feilmelding2 =
                "Nøkkelen må inneholde engelske bokstaver fra A til Z.";

        let midlertidigResultat = "";
        let nokkelPosisjon = 0;

        for (let i = 0; i < tekst2.length; i++) {
            const b = tekst2[i];

            if (b === " ") {
                midlertidigResultat += " ";
                continue;
            }

            const u = b.toUpperCase();
            const pos = ALFABET.indexOf(u);

            if (pos === -1)
                return feilmelding2 =
                    "Kun engelske bokstaver og mellomrom er tillatt.";

            const nokkelverdi =
                ALFABET.indexOf(
                    nokkel[nokkelPosisjon % nokkel.length]
                );

            let nyPos = (pos + nokkelverdi * retning) % 26;
            if (nyPos < 0) nyPos += 26;

            midlertidigResultat +=
                b === u
                    ? ALFABET[nyPos]
                    : ALFABET[nyPos].toLowerCase();

            nokkelPosisjon++;
        }

        resultat2 = midlertidigResultat;
    }

    // ONE-TIME PAD: GENERER TILFELDIG NØKKEL
    function lagNokkel(lengde) {
        let nyNokkel = "";

        for (let i = 0; i < lengde; i++) {
            const tilfeldigTall = Math.floor(Math.random() * 26);
            nyNokkel += ALFABET[tilfeldigTall];
        }

        return nyNokkel;
    }

    // ONE-TIME PAD: KRYPTERING OG DEKRYPTERING
    function kjorOnepad(retning) {
        feilmelding3 = "";
        resultat3 = "";

        if (!tekst3.trim())
            return feilmelding3 = "Tekstfeltet kan ikke være tomt.";

        // Kontroller tekst: kun A-Z, a-z og mellomrom.
        for (const b of tekst3) {
            if (b !== " " && ALFABET.indexOf(b.toUpperCase()) === -1) {
                return feilmelding3 =
                    "Teksten kan bare inneholde engelske bokstaver og mellomrom.";
            }
        }

        const antallBokstaver = tekst3.replace(/ /g, "").length;
        let aktivNokkel;

        if (retning === 1) {
            // Generer en ny tilfeldig nøkkel ved kryptering.
            aktivNokkel = lagNokkel(antallBokstaver);
            nokkel3 = aktivNokkel;
        } else {
            // Ved dekryptering brukes nøkkelen brukeren skriver inn.
            aktivNokkel = nokkel3.toUpperCase();

            if (!/^[A-Z]+$/.test(aktivNokkel)) {
                return feilmelding3 =
                    "Nøkkelen må bare inneholde bokstaver fra A til Z.";
            }

            if (aktivNokkel.length !== antallBokstaver) {
                return feilmelding3 =
                    "Nøkkelen må ha like mange bokstaver som teksten uten mellomrom.";
            }
        }

        let midlertidigResultat = "";
        let nokkelPosisjon = 0;

        for (let i = 0; i < tekst3.length; i++) {
            const b = tekst3[i];

            // Mellomrom beholdes og bruker ikke nøkkelbokstaver.
            if (b === " ") {
                midlertidigResultat += " ";
                continue;
            }

            const u = b.toUpperCase();
            const pos = ALFABET.indexOf(u);
            const nokkelverdi =
                ALFABET.indexOf(aktivNokkel[nokkelPosisjon]);

            let nyPos = (pos + nokkelverdi * retning) % 26;

            if (nyPos < 0) nyPos += 26;

            // Behold store og små bokstaver.
            midlertidigResultat +=
                b === u
                    ? ALFABET[nyPos]
                    : ALFABET[nyPos].toLowerCase();

            nokkelPosisjon++;
        }

        resultat3 = midlertidigResultat;
    }
</script>

<h2>Caesar Cipher</h2>

<label>
    Tekst:
    <input type="text" bind:value={tekst}>
</label>

<label>
    Nøkkeltall (0–25):
    <input type="number" min="0" max="25" bind:value={nokkel}>
</label>

<button onclick={() => kjorCaesar(1)}>Krypter</button>
<button onclick={() => kjorCaesar(-1)}>Dekrypter</button>

{#if feilmelding}
    <p>{feilmelding}</p>
{/if}

{#if resultat}
    <p>Resultat: <strong>{resultat}</strong></p>
{/if}


<!-- VIGENERE -->
<h2>Vigenère</h2>

<label>
    Tekst:
    <input type="text" bind:value={tekst2}>
</label>

<label>
    Nøkkelord:
    <input type="text" bind:value={nokkel2}>
</label>

<button onclick={() => kjorvigenere(1)}>Krypter</button>
<button onclick={() => kjorvigenere(-1)}>Dekrypter</button>

{#if feilmelding2}
    <p>{feilmelding2}</p>
{/if}

{#if resultat2}
    <p>Resultat: <strong>{resultat2}</strong></p>
{/if}



<h2>One-Time Pad</h2>

<label>
Text: 
    <input
        type="text"
        bind:value={tekst3}
    >
</label>

<label>
    Nøkkelord:
    <input
        type="text"
        bind:value={nokkel3}
    >
</label>

<button onclick={() => kjorOnepad(1)}>Krypter</button>
<button onclick={() => kjorOnepad(-1)}>Dekrypter</button>

{#if feilmelding3}
    <p>{feilmelding3}</p>
{/if}

{#if nokkel3}
    <p><strong>{nokkel3}</strong></p>
{/if}

{#if resultat3}
    <p>
        Resultat:
        <strong>{resultat3}</strong></p>
{/if}
