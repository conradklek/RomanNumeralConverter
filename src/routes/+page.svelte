<script>
  const numbers = [7, 8, 9, 4, 5, 6, 1, 2, 3, 0]
  $: input = ""
  $: value = Number(input)
  $: roman = convert(value)
  function convert(number) {
    if (!number) return ""
    if (number < 1 || number > 3999) return "Must be between 1 and 3999"
    if (`${number}`[0] === "0") return convert(Number(`${number}`.slice(1)))
    let roman = ""
    const romanNumList = { M: 1000, CM: 900, D: 500, CD: 400, C: 100, XC: 90, L: 50, XV: 40, X: 10, IX: 9, V: 5, IV: 4, I: 1 }
    let a
    for (let key in romanNumList) {
      a = Math.floor(number / romanNumList[key])
      if (a >= 0) {
        for (let i = 0; i < a; i++) {
          roman += key
        }
      }
      number = number % romanNumList[key]
    }
    return roman
  }
</script>

<main>
  <header>
    <h1>
      Roman Numeral Converter
    </h1>
  </header>
  <aside>
    <h2>
      Description:
      <div>
        Enter a number 
        <span class:error={(value && (value < 1 || value > 3999))}>
          between 1 and 3999
        </span> 
        to convert it into a Roman Numeral.
      </div>
    </h2>
  </aside>
  <form on:submit|preventDefault>
    <label for=input>
      Input:
      <input id=input type=number bind:value={input} />
    </label>
    <label for=output>
      Output:
      <div>
        {(input && (input > 0 && input < 3999)) ? roman : ""}
      </div>
    </label>
    <div>
      {#each numbers as number}
      <div>
        <button on:click={() => input = `${input || ""}${number}`}>
          {number}
        </button>
      </div>
      {/each}
      <div>
        <button disabled={!input} on:click={() => input = ""}>
          Clear
        </button>
      </div>
    </div>
  </form>
</main>