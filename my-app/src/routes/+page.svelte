<script>
    let tekst = $state("");
    let nokkel = $state(0);
    let resultat = $state("");
    let feilmelding = $state("");

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
</script>

<h2>Caesar Cipher</h2>
<label>Tekst: <input type="text" bind:value={tekst}></label>
<label>Nøkkel: <input type="number" bind:value={nokkel}></label>

<button onclick={() => kjorCaesar(1)}>Krypter</button>
<button onclick={() => kjorCaesar(-1)}>dekrypter</button>

{#if feilmelding} <p>{feilmelding}</p>{/if}
{#if resultat} <p>Resulatat: <strong>{resultat}</strong></p>{/if}