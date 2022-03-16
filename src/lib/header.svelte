<header class="header-banner">
  <img src="/static/cagers-logo-sm.png" alt="site logo" />
  <h1>CAGERS BASKETBALL</h1>
  <img src="/static/cagers-logo-sm.png" alt="site logo" />
</header>

<section class="tagline">
  <h3>The BEST never REST!</h3>
</section>

<style>
  .header-banner {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 10px;
    background: white;
  }
  .header-banner img {
    width: 50px;
  }
  .header-banner h1 {
    color: var(--dark);
  }
  .tagline {
    display: flex;
    height: 100px;
    background: var(--primary);
    justify-content: center;
    align-items: center;
  }
  .tagline h3 {
    color: white;
    font-size: 1.3em;
  }
  @media (max-width: 575px) {
    .header-banner h1 {
      font-size: 1.5em;
    }
  }
  @media (max-width: 400px) {
    .header-banner h1 {
      font-size: 1.15em;
    }
  }
</style>
