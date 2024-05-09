<script>
    const speeds = [
        {multiplier: 1, text: "Key/s"},
        {multiplier: 1000, text: "Kkey/s"},
        {multiplier: 1000000, text: "Mkey/s"},
        {multiplier: 1000000000, text: "Gkey/s"},
        {multiplier: 1000000000000, text: "Tkey/s"},
        {multiplier: 1000000000000000, text: "Pkey/s"}
    ];

    let selectedSpeed = speeds[0];
    let inputRange = "";
    let inputSpeed = "";
    let output = "";

    const calculateTime = () => {
        const hexToDec = (hexString) => {
            return parseInt(hexString, 16);
        }

        let hexRange = inputRange.split(":");
        let startHex = hexToDec(hexRange[0]);
        let endHex = hexToDec(hexRange[1]);
        let rangeSize = endHex - startHex + 1;
        let speed = inputSpeed * selectedSpeed.multiplier

        let timeInSeconds = rangeSize / speed;
        let timeInMinutes = timeInSeconds / 60;
        let timeInHours = timeInMinutes / 60;
        let timeInDays = timeInHours / 24;
        let timeInYears = timeInDays / 365;

        let outputText = "";
        if (timeInYears >= 1) {
            outputText += Math.floor(timeInYears) + " years ";
            timeInDays %= 365;
        }
        if (timeInDays >= 1) {
            outputText += Math.floor(timeInDays) + " days ";
            timeInHours %= 24;
        }
        if (timeInHours >= 1) {
            outputText += Math.floor(timeInHours) + " hours ";
            timeInMinutes %= 60;
        }
        if (timeInMinutes >= 1) {
            outputText += Math.floor(timeInMinutes) + " minutes ";
            timeInSeconds %= 60;
        }
        if (timeInSeconds < 1) {
            outputText += " 0 seconds";
        } else if (timeInSeconds >= 1) {
            outputText += Math.floor(timeInSeconds) + " seconds";
        }
        output = outputText;
    }
</script>

<div class="container-2">
    <h2>Speed</h2>
    <p>Private Key Range (hex):</p>
    <input type="text" id="input-1" bind:value={inputRange} placeholder="2D32B12D3415A6DD2:2D32B3461309BD9AD">
    <p>Your speed (dec):</p>

    <input type="text" id="input-2" bind:value={inputSpeed} placeholder="650">
    <select bind:value={selectedSpeed}>

        {#each speeds as speed}
            <option value={speed}>
                {speed.text}
            </option>
        {/each}

    </select>

    <div class="output">
        <p>time to reach 100%:</p>
        <span id="output1">{output}</span>
    </div>

    <button on:click={calculateTime}>Calc</button>
</div>

<style>
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

    .container-2 input::placeholder {
        color: #4b4b4b;
        font-family: DejaVu Sans Mono;
    }

    .container-2 {
        max-width: 800px;
        margin: auto;
        margin-top: 100px;
        margin-bottom: 200px;
        padding: 20px;
        background: rgba(255, 255, 255, 0);
        border-radius: 16px;
        box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
        backdrop-filter: blur(4.2px);
        -webkit-backdrop-filter: blur(4.2px);
        border: 1px solid rgba(255, 255, 255, 0.13);
    }

    .container-2 h2 {
        text-align: center;
        margin-bottom: 30px;
        margin-top: 0;
        font-family: DejaVu Sans Mono;
        font-size: 35px;
        font-weight: normal;
    }

    #input-2 {
        width: 80%;
    }

    #input-2::placeholder,
    #input-2 {
        text-indent: 26%;
    }

    select {
        display: inline-block;
        background-color: rgba(0, 0, 0, 0);
        border-radius: 8px;
        padding: 8px 12px;
        margin: 0;
        outline: none;
        font-family: DejaVu Sans Mono;
        width: 17%;
    }
</style>