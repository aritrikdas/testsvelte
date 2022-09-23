<script>
  import { Router, Route, navigate, Link } from "svelte-navigator";
  import Auctiondetails from "./auctiondetails.svelte";
  import Login from "./Login.svelte";
  import Navbar from "./Navbar.svelte";
  import Auctionlist from "./Auctionlist.svelte";
  import {
    toasts,
    ToastContainer,
    FlatToast,
    BootstrapToast,
  } from "svelte-toasts";

  const showToast = () => {
    const toast = toasts.add({
      title: "Success",
      description: "Form submitted successfully",
      duration: 3000,
      placement: "bottom-right",
      type: "info",
      theme: "dark",
      showProgress: true,
      onClick: () => {},
      onRemove: () => {},
    });
  };
</script>

<Router>
  <Route path="/">
    <Navbar />
    <Login />
  </Route>
  <Route path="/auctiondetails">
    <Navbar />
    <Auctiondetails />
  </Route>
  <Route path="/auctionlist">
    <Navbar />
    <Auctionlist />
  </Route>
</Router>

<ToastContainer placement="bottom-right" let:data>
  <FlatToast {data} />
</ToastContainer>

<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>
