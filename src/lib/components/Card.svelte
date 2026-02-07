<script>
  let { icon = "", title = "", children } = $props();
</script>

<div class="card">
  {#if icon}
    <i class={icon}></i>
  {/if}
  {#if title}
    <h3>{title}</h3>
  {/if}
  {@render children()}
</div>

<style>
  .card {
    background: rgb(252, 242, 244);
    padding: 25px;
    border-radius: 16px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
    transition: transform 0.3s ease;
    animation: fadeIn 0.8s ease forwards;
  }

  .card:hover {
    transform: translateY(-8px);
  }

  .card i {
    font-size: 2.2rem;
    color: var(--primary);
    margin-bottom: 15px;
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
</style>
