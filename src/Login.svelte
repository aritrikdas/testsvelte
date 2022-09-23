<script>
  import axios from "axios";
  import { Router, Route, navigate } from "svelte-navigator";
  import {
    toasts,
    ToastContainer,
    FlatToast,
    BootstrapToast,
  } from "svelte-toasts";
  let spinnerLoading = false;

  let loginUser = {};

  const handleLogin = async () => {
    try {
      spinnerLoading = true;
      console.log("loginUser", loginUser);
      const res = await axios.post(
        "https://fu51ye2x3g.execute-api.us-east-1.amazonaws.com/dev/auth/login",
        loginUser
      );
      console.log("res", res);

      if (res.data.message) {
        console.log("token", res.data.data.AccessToken);
        localStorage.setItem("AccessToken", res.data.data.AccessToken);
        spinnerLoading = false;
        navigate("/auctionlist");
        const toast = toasts.add({
          title: "Success",
          description: "Login Successfull!",
          duration: 3000,
          placement: "bottom-right",
          type: "success",
          theme: "dark",
          showProgress: true,
          onClick: () => {},
          onRemove: () => {},
        });
      }
    } catch (err) {
      spinnerLoading = false;
      const toast = toasts.add({
        title: "Error",
        description: "Authentication Failed!",
        duration: 3000,
        placement: "bottom-right",
        type: "error",
        theme: "dark",
        showProgress: true,
        onClick: () => {},
        onRemove: () => {},
      });
    }
  };
</script>

<section class="h-full gradient-form md:h-screen">
  <div class="container py-12 px-6 h-full">
    <div
      class="flex justify-center items-center flex-wrap h-full g-6 text-gray-800"
    >
      <div class="xl:w-12/12">
        <div class="block bg-white shadow-lg rounded-lg border">
          <div class="lg:flex lg:flex-wrap g-0">
            <div class=" px-4 md:px-0">
              <div class="md:p-6 md:mx-6">
                <div class="text-center">
                  <img
                    class="mx-auto w-48"
                    src="https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-login-form/lotus.webp"
                    alt="logo"
                  />
                  <h4 class="text-xl font-semibold mt-1 mb-12 pb-1">
                    We are The Svelte Team
                  </h4>
                </div>
                <div class="md:w-80">
                  <p class="mb-4">Please login to your account</p>
                  <div class="mb-4">
                    <input
                      type="text"
                      class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                      id="exampleFormControlInput1"
                      placeholder="Username"
                      bind:value={loginUser.email}
                    />
                  </div>
                  <div class="mb-4">
                    <input
                      type="password"
                      class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                      id="exampleFormControlInput1"
                      placeholder="Password"
                      bind:value={loginUser.password}
                    />
                  </div>
                  <div class="text-center pt-1 mb-12 pb-1">
                    <button
                      class="inline-block px-6 py-0.5 md:h-12 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-700 hover:shadow-lg focus:shadow-lg focus:outline-none focus:ring-0 active:shadow-lg transition duration-150 ease-in-out w-full mb-3"
                      type="button"
                      data-mdb-ripple="true"
                      data-mdb-ripple-color="light"
                      style="
                          background: linear-gradient(
                            to right,
                            #ee7724,
                            #d8363a,
                            #dd3675,
                            #b44593
                          );
                        "
                      on:click={() => handleLogin()}
                    >
                      {#if spinnerLoading}
                        <div class="flex justify-center items-center">
                          <div
                            class="spinner-border animate-spin inline-block w-8 h-8 border-4 rounded-full"
                            role="status"
                          >
                            <span class="visually-hidden">Loading...</span>
                          </div>
                        </div>
                      {:else}
                        <span>Log in</span>
                      {/if}
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<ToastContainer let:data>
  <FlatToast {data} />
</ToastContainer>
