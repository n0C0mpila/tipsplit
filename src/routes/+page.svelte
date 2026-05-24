<script>
  let monto = $state("")
  let porcentaje = $state(15)
  let personas = $state(1)

  let propina = $derived(monto ? (parseFloat(monto) * porcentaje) / 100 : 0)
  let total = $derived(monto ? parseFloat(monto) + propina : 0)
  let porPersona = $derived(personas > 0 ? total / personas : 0)

  function reset() {
    monto = ""
    porcentaje = 15
    personas = 1
  }
</script>

<div class="page">

  <div class="card">

    <div class="card-header">
      <div class="logo">
        <i class="ti ti-receipt-2"></i>
      </div>
      <div>
        <h1>TipSplit</h1>
        <p>Calculá tu propina al instante</p>
      </div>
    </div>

    <div class="inputs">

      <div class="field">
        <!-- svelte-ignore a11y_label_has_associated_control -->
        <label>Monto de la cuenta</label>
        <div class="input-box">
          <span>$</span>
          <input
            type="number"
            bind:value={monto}
            min="0"
            placeholder="0.00"
          />
        </div>
      </div>

      <div class="field">
        <div class="field-header">
          <!-- svelte-ignore a11y_label_has_associated_control -->
          <label>Propina</label>
          <span class="badge">{porcentaje}%</span>
        </div>
        <input type="range" bind:value={porcentaje} min="0" max="30" />
        <div class="chips">
          {#each [10, 15, 18, 20, 25] as op}
            <button
              class="chip {porcentaje === op ? 'chip-active' : ''}"
              onclick={() => porcentaje = op}
            >
              {op}%
            </button>
          {/each}
        </div>
      </div>

      <div class="field">
        <!-- svelte-ignore a11y_label_has_associated_control -->
        <label>Personas</label>
        <div class="stepper">
          <!-- svelte-ignore a11y_consider_explicit_label -->
          <button class="step-btn" onclick={() => personas = Math.max(1, personas - 1)}>
            <i class="ti ti-minus"></i>
          </button>
          <span class="step-value">{personas}</span>
          <!-- svelte-ignore a11y_consider_explicit_label -->
          <button class="step-btn" onclick={() => personas = personas + 1}>
            <i class="ti ti-plus"></i>
          </button>
        </div>
      </div>

    </div>

    <div class="results">
      <div class="result-row">
        <span>Propina</span>
        <span>${propina.toFixed(2)}</span>
      </div>
      <div class="result-row">
        <span>Total</span>
        <span>${total.toFixed(2)}</span>
      </div>
      <div class="result-highlight">
        <div>
          <p class="highlight-label">Por persona</p>
          <p class="highlight-sub">Entre {personas} {personas === 1 ? 'persona' : 'personas'}</p>
        </div>
        <span class="highlight-amount">${porPersona.toFixed(2)}</span>
      </div>
    </div>

    <button class="reset-btn" onclick={reset}>
      <i class="ti ti-refresh"></i>
      Nueva cuenta
    </button>

  </div>

</div>

<style>
  :global(*, *::before, *::after) {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  :global(body) {
    background: #080f0d;
    font-family: 'Inter', system-ui, sans-serif;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .page {
    width: 100%;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 24px 16px;
    background: radial-gradient(ellipse at top, #0a2a1a 0%, #080f0d 60%);
  }

  .card {
    background: #0f1a16;
    border: 1px solid #1a3028;
    border-radius: 24px;
    padding: 32px;
    width: 100%;
    max-width: 440px;
    display: flex;
    flex-direction: column;
    gap: 28px;
    box-shadow: 0 25px 60px rgba(0,0,0,0.5);
  }

  .card-header {
    display: flex;
    align-items: center;
    gap: 14px;
  }

  .logo {
    width: 48px;
    height: 48px;
    background: linear-gradient(135deg, #10b981, #059669);
    border-radius: 14px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  .logo i {
    font-size: 24px;
    color: white;
  }

  .card-header h1 {
    font-size: 22px;
    font-weight: 800;
    color: #e8f5ee;
    letter-spacing: -0.02em;
  }

  .card-header p {
    font-size: 13px;
    color: #5a7a6a;
    margin-top: 2px;
  }

  .inputs {
    display: flex;
    flex-direction: column;
    gap: 22px;
  }

  .field {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .field-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  label {
    font-size: 12px;
    font-weight: 600;
    color: #5a7a6a;
    text-transform: uppercase;
    letter-spacing: 0.06em;
  }

  .badge {
    background: linear-gradient(135deg, #10b981, #059669);
    color: white;
    font-size: 12px;
    font-weight: 700;
    padding: 3px 10px;
    border-radius: 99px;
  }

  .input-box {
    display: flex;
    align-items: center;
    background: #091510;
    border: 1px solid #1a3028;
    border-radius: 12px;
    padding: 0 16px;
    transition: border-color 0.2s;
  }

  .input-box:focus-within {
    border-color: #10b981;
    box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.1);
  }

  .input-box span {
    color: #5a7a6a;
    font-size: 18px;
    font-weight: 600;
    margin-right: 8px;
  }

  input[type="number"] {
    flex: 1;
    background: transparent;
    border: none;
    outline: none;
    color: #e8f5ee;
    font-size: 20px;
    font-weight: 600;
    padding: 14px 0;
    font-family: inherit;
  }

  input[type="range"] {
    width: 100%;
    accent-color: #10b981;
    cursor: pointer;
    height: 4px;
  }

  .chips {
    display: flex;
    gap: 6px;
  }

  .chip {
    flex: 1;
    padding: 7px 4px;
    border-radius: 8px;
    border: 1px solid #1a3028;
    background: #091510;
    color: #5a7a6a;
    font-size: 12px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.15s;
    font-family: inherit;
  }

  .chip:hover {
    border-color: #10b981;
    color: #10b981;
  }

  .chip-active {
    background: linear-gradient(135deg, #10b981, #059669);
    border-color: transparent;
    color: white !important;
  }

  .stepper {
    display: flex;
    align-items: center;
    background: #091510;
    border: 1px solid #1a3028;
    border-radius: 12px;
    overflow: hidden;
    width: fit-content;
  }

  .step-btn {
    background: transparent;
    border: none;
    color: #5a7a6a;
    padding: 12px 18px;
    cursor: pointer;
    font-size: 16px;
    transition: all 0.15s;
    font-family: inherit;
    display: flex;
    align-items: center;
  }

  .step-btn:hover {
    background: #1a3028;
    color: #e8f5ee;
  }

  .step-value {
    color: #e8f5ee;
    font-size: 18px;
    font-weight: 700;
    min-width: 48px;
    text-align: center;
  }

  .results {
    background: #091510;
    border: 1px solid #1a3028;
    border-radius: 16px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  .result-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 14px;
  }

  .result-row span:first-child {
    color: #5a7a6a;
  }

  .result-row span:last-child {
    color: #a8d4bc;
    font-weight: 600;
  }

  .result-highlight {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: linear-gradient(135deg, #0d2018, #0a2a1a);
    border: 1px solid #1a4030;
    border-radius: 12px;
    padding: 16px;
    margin-top: 4px;
  }

  .highlight-label {
    font-size: 13px;
    font-weight: 700;
    color: #a8d4bc;
  }

  .highlight-sub {
    font-size: 11px;
    color: #5a7a6a;
    margin-top: 3px;
  }

  .highlight-amount {
    font-size: 28px;
    font-weight: 800;
    background: linear-gradient(135deg, #10b981, #059669);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .reset-btn {
    width: 100%;
    background: transparent;
    border: 1px solid #1a3028;
    border-radius: 12px;
    padding: 13px;
    color: #5a7a6a;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 7px;
    transition: all 0.15s;
    font-family: inherit;
  }

  .reset-btn:hover {
    border-color: #10b981;
    color: #10b981;
    background: rgba(16, 185, 129, 0.05);
  }
</style>