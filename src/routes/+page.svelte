<script>
    let ratios = '';
    let reference = '';
    let adjustedRatios = [];

    function gcd(a, b) {
        return b ? gcd(b, a % b) : a;
    }

    function parseFraction(fraction) {
        const [numerator, denominator] = fraction.split('/').map(Number);
        return { numerator, denominator };
    }

    function simplifyFraction(numerator, denominator) {
        const divisor = gcd(numerator, denominator);
        return `${numerator / divisor}/${denominator / divisor}`;
    }

    function calculateRatios() {
        const ratioList = ratios.split(',').map(r => r.trim());
        const referenceValue = parseFraction(reference);

        adjustedRatios = ratioList.map(ratio => {
            const value = parseFraction(ratio);
            const adjustedNumerator = value.numerator * referenceValue.denominator;
            const adjustedDenominator = value.denominator * referenceValue.numerator;
            const simplified = simplifyFraction(adjustedNumerator, adjustedDenominator);
            return `${ratio} (adjusted: ${simplified})`;
        });
    }
</script>

<h2>Alec's Cool Tuning System Ratio Calculator</h2>

<div>
    <label for="ratios">Enter Ratios (comma-separated):</label><br>
    <input type="text" bind:value={ratios} placeholder="e.g. 1/1, 64/63, 12/11, 9/8, 2/1">
</div>

<div>
    <label for="reference">New Reference Ratio:</label><br>
    <input type="text" bind:value={reference} placeholder="e.g. 64/63">
</div>

<button on:click={calculateRatios}>Calculate</button>

{#if adjustedRatios.length > 0}
    <h3>Adjusted Ratios:</h3>
    <ul>
        {#each adjustedRatios as ratio}
            <li>{ratio}</li>
        {/each}
    </ul>
{/if}
