<script>
    let tekst = $state("");
    let nokkel = $state(0);
    let resultat = $state("");
    let feilmelding = $state("");

    let tekst2 = $state("");
    let nokkel2 = $state("");
    let resultat2 = $state("");
    let feilmelding2 = $state("");

    const ALFABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

    function kjorCaesar(retning) {
        feilmelding = "";
        resultat = "";

        if (!tekst.trim()) return feilmelding = " tekstfeltet kan ikke være tomt";
     
        if (nokkel=== null || nokkel < 0 || nokkel > 25) return feilmelding = "Nøkkel må være 0-25."

        let midlertidigResultat = "";

        for (let i = 0; i < tekst.length; i++) {
            const b = tekst[i]

            if (b === " ") {
                midlertidigResultat += " ";
                continue;
            }

            const u = b.toUpperCase();
            const pos = ALFABET.indexOf(u);

            if(pos === -1) {
                return feilmelding = "kun engelsk bokstaver"
            }

            let nyPos = (pos + (nokkel * retning)) % 26;
            if (nyPos < 0) nyPos += 26;

            midlertidigResultat += b === u ? ALFABET[nyPos] : ALFABET[nyPos].toLowerCase();
            resultat = midlertidigResultat;

        }
    }
    //DETTE ER FOR VIGENERE
    function kjorvigenere(retning) {
        feilmelding2 = "";
        resultat2 = "";

        if (!tekst2.trim())
            return feilmelding2 = " tekstfeltet kan ikke være tomt";

        const nokkel = nokkel2.toUpperCase();

        if (!/^[A-Z]+$/.test(nokkel))
            return feilmelding2 =   "kun engelsk bokstaver";

        let midlertidigResultat = "";

        let nokkelPosisjon = 0; 

        for (let i = 0; i < tekst2.length; i++) {
            const b = tekst2[i]

            if (b === " ") {
                midlertidigResultat += " ";
                continue;
            }

            const u = b.toUpperCase();
            const pos = ALFABET.indexOf(u);

            if(pos === -1) {
                feilmelding2 = "kun engelsk bokstaver"
                return;
            }

            const nokkelverdi =
                ALFABET.indexOf(nokkel[nokkelPosisjon % nokkel.length]);

            let nyPos = (pos + (nokkelverdi * retning)) % 26;

            if (nyPos < 0) nyPos += 26;

            midlertidigResultat +=
                b === u 
                    ? ALFABET[nyPos] 
                    : ALFABET[nyPos].toLowerCase();

            nokkelPosisjon++;

        }

        resultat2 = midlertidigResultat;
    }
</script>

<h2>Caesar Cipher</h2>
<label>Tekst: <input type="text" bind:value={tekst}></label>
<label>Nøkkeltall: <input type="number" bind:value={nokkel}></label>

<button onclick={() => kjorCaesar(1)}>Krypter</button>
<button onclick={() => kjorCaesar(-1)}>dekrypter</button>

{#if feilmelding} <p>{feilmelding}</p>{/if}
{#if resultat} <p>Resulatat: <strong>{resultat}</strong></p>{/if}

<!-- DETTE ER FOR VIGENERE -->
<h2>Vigenere</h2>
<label>Tekst: <input type="text" bind:value={tekst2}></label>
<label>Nøkkelord: <input type="text" bind:value={nokkel2}></label>

<button onclick={() => kjorvigenere(1)}>Krypter</button>
<button onclick={() => kjorvigenere(-1)}>dekrypter</button>

{#if feilmelding2} <p>{feilmelding2}</p>{/if}
{#if resultat2} <p>Resulatat: <strong>{resultat2}</strong></p>{/if}