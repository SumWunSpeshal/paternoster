<script>
  import '../main.css';
</script>

<header>Header</header>
<slot />
<footer>
  Footer
  <br>
  <p>Sehr langer Footer um zu demonstrieren wie der overscroll aussieht.</p>
</footer>

<style>
  header {
    background-color: lightblue;
    padding: 1rem;
  }

  footer {
    background-color: lightyellow;
    padding: 1rem;
    height: 150vh;
  }

  footer p {
    width: min-content;
  }
</style>
