<script>
  import { Link } from "svelte-navigator";
</script>

<nav class="navbar navbar-expand-sm navbar-dark bg-dark">
  <div class="container-fluid">
    <Link to="/" class="navbar-brand"
      ><img
        class="mr-2"
        src="https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-login-form/lotus.webp"
        style="height: 20px"
        alt=""
        loading="lazy"
      /></Link
    >
    <button
      class="navbar-toggler"
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#mynavbar"
    >
      <span class="navbar-toggler-icon" />
    </button>
    <div class="collapse navbar-collapse" id="mynavbar">
      <ul class="navbar-nav me-auto">
        <li class="nav-item">
          <Link to="/" class="nav-link active">Svelte</Link>
        </li>
      </ul>
      <form class="d-flex">
        <input class="form-control me-2" type="text" placeholder="Search" />
        <button class="btn btn-primary" type="button">Search</button>
      </form>
    </div>
  </div>
</nav>
