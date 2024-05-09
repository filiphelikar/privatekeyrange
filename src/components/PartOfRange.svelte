<script>
    let bigRange = ""
    let divisor = ""
    let partOfRange = ""
    let resultRange = ""
    let outputRandomNumber = ""
    let outputPercentage = ""

    const updateOutputs = () => {
        partOfRange = partOfRange + '';
        let partOfRangeOrRandom = 0;

        if (partOfRange.trim() === '' || isNaN(partOfRange)) {
            partOfRangeOrRandom = Math.floor(Math.random() * parseInt(divisor)) + 1;
        } else {
            partOfRangeOrRandom = +partOfRange
        }

        const result = calculateRangeAndRandom(bigRange, divisor, partOfRangeOrRandom);

        resultRange = result.start + ":" + result.end;
        outputRandomNumber = result.random;
        outputPercentage = result.percentage;
    }

    const calculateRangeAndRandom = (bigRange, divisor, numberOfCalculationsOrRandom) => {
        const [startHex, endHex] = bigRange.split(':');

        const start = BigInt('0x' + startHex);
        const end = BigInt('0x' + endHex);
        const decimalRange = end - start;

        const onePart = decimalRange / BigInt(divisor);

        const newStart = start + (onePart * BigInt(numberOfCalculationsOrRandom - 1));
        const newEnd = newStart + onePart;

        const percentageS = ((Number(newStart - start) * 100) / Number(decimalRange)).toFixed(10);
        const percentageE = ((Number(newEnd - start) * 100) / Number(decimalRange)).toFixed(10);

        const totalPercentage = ((Number(newEnd - newStart) * 100) / Number(decimalRange)).toFixed(10);

        return {
            start: newStart.toString(16).toUpperCase(),
            end: newEnd.toString(16).toUpperCase(),
            random: numberOfCalculationsOrRandom,
            percentage: percentageS + "%" + " - " + percentageE + "%" + "   Total:  " + totalPercentage + "%"
        };
    }
</script>

<div class="container">
    <h2>Part of the range</h2>
    <p>Private Key Range (hex):</p>
    <input type="text" bind:value={bigRange} placeholder="20000000000000000:3ffffffffffffffff">

    <p>Divisor (dec):</p>
    <input type="number" bind:value={divisor} placeholder="1000000">

    <p> Manual number in Key Range or Random (dec):</p>
    <input type="number" bind:value={partOfRange} placeholder="Enter starting number or leave blank for random">


    <div class="output">
        <p>Calculated Private Key Range (hex):</p>
        <span>{resultRange}</span>
    </div>

    <div class="output">
        <p>Random number (dec):</p>
        <span>{outputRandomNumber}</span>
    </div>

    <div class="output">
        <p>Percentage position in original Key Range:</p>
        <span>{outputPercentage}</span><br>
    </div>

    <button on:click={updateOutputs}>Calculate</button>
</div>

<style>
    h2 {
        text-align: center;
        margin-bottom: 30px;
        margin-top: 0;
        font-family: DejaVu Sans Mono;
        font-size: 35px;
        font-weight: normal;
    }

    .container {
        max-width: 800px;
        margin: auto;
        margin-top: 100px;
        padding: 20px;
        background: rgba(255, 255, 255, 0);
        border-radius: 16px;
        box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
        backdrop-filter: blur(4.2px);
        -webkit-backdrop-filter: blur(4.2px);
        border: 1px solid rgba(255, 255, 255, 0.13);
    }

    p {
        display: block;
        font-size: 21px;

        margin-bottom: 10px;
        text-align: center;
    }

    .output {
        text-align: center;
        font-size: 16px;
        margin-top: 20px;

    }

    button {
        display: block;
        font-size: 16px;
        padding: 10px 20px;
        background: rgba(255, 255, 255, 0);
        border-radius: 8px;
        box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
        backdrop-filter: blur(4.2px);
        -webkit-backdrop-filter: blur(4.2px);
        border: 1px solid rgba(255, 255, 255, 0.13);

        cursor: pointer;
        margin: 0 auto;
        margin-top: 15px;
        font-family: DejaVu Sans Mono;
    }

    .container input::placeholder {
        color: #4b4b4b;
        font-family: DejaVu Sans Mono;
    }
</style>