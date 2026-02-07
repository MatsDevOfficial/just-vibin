<script>
  let grams = $state(0);
  let price = $state(20);
  let result = $derived(grams && price ? (grams / 1000 * price).toFixed(2) : '0.00');
</script>

<div class="calculator-card">
  <div class="input-group">
    <label for="grams">Filament used (grams)</label>
    <input id="grams" type="number" bind:value={grams} placeholder="e.g. 50">
  </div>

  <div class="input-group">
    <label for="price">Filament price ($/kg)</label>
    <input id="price" type="number" bind:value={price} placeholder="e.g. 20">
  </div>

  <div class="result">
    <p>Estimated print cost: <strong>${result}</strong></p>
  </div>
</div>

<style>
  .calculator-card {
    padding: 10px;
  }
  .input-group {
    margin-bottom: 15px;
  }
  label {
    display: block;
    margin-bottom: 5px;
    font-weight: 600;
  }
  input {
    width: 100%;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 1rem;
  }
  .result {
    margin-top: 20px;
    padding: 15px;
    background: #e6f7f2;
    border-radius: 8px;
    border: 1px solid var(--primary);
  }
  .result p {
    margin: 0;
    color: var(--dark);
  }
  strong {
    color: var(--primary);
    font-size: 1.2rem;
  }
</style>
