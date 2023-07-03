<script>
  let component = null;

  function onScroll(e) {
    component.scrollBy({
      top: e.deltaY,
      behavior: 'smooth'
    })
  }
</script>

<div class="paternoster" on:wheel={onScroll}>
  <div>
    <div class="sidebar" bind:this={component}>
      <slot name="sidebar" />
    </div>
  </div>
  <div>
    <div class="content">
      <slot name="content"/>
    </div>
  </div>
</div>

<style>
  .paternoster {
    max-width: 960px;
    margin-inline: auto;
    margin-block: 4rem;

    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(0, 1fr));
    gap: 2rem;
  }

  .content,
  .sidebar {
    position: sticky;
    top: 0;
  }

  .sidebar {
    border: 1px solid red;
    overflow-y: auto;
    max-height: 100vh;
  }
</style>