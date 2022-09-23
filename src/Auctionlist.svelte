<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import { navigate } from "svelte-navigator";
  let spinnerLoading = false;

  let auctiondata = [];

  onMount(async () => {
    try {
      spinnerLoading = true;
      const response = await axios.get(
        "https://fu51ye2x3g.execute-api.us-east-1.amazonaws.com/dev/auctions/upcoming",
        {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("AccessToken"),
          },
        }
      );
      spinnerLoading = false;
      auctiondata = response.data.data;
      console.log("puas", response.data.data);
    } catch (err) {
      console.log("Auction List Err", err);
    }
  });

  function gotoDetails(id) {
    navigate("/auctiondetails?auctionid=" + id);
  }
</script>

<div class="container md:px-32 md:my-12">
  <h2 class="text-3xl font-bold mb-12 pb-4 text-center">Latest Auctions</h2>
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
    <section class="mb-32 text-gray-800 text-center">
      <div class="grid lg:grid-cols-3 gap-6 xl:gap-x-12">
        {#each auctiondata as item}
          <div>
            <div class="mb-6 lg:mb-0">
              <div class="relative block bg-white rounded-lg shadow-lg">
                <div class="flex">
                  <div
                    class="relative overflow-hidden bg-no-repeat bg-cover shadow-lg rounded-lg mx-4 -mt-4"
                    data-mdb-ripple="true"
                    data-mdb-ripple-color="light"
                  >
                    <img src={item.vehicle.photoUrl} alt="" class="w-full" />
                    <a href="#!">
                      <div
                        class="absolute top-0 right-0 bottom-0 left-0 w-full h-full overflow-hidden bg-fixed opacity-0 hover:opacity-100 transition duration-300 ease-in-out"
                        style="background-color: rgba(251, 251, 251, 0.15)"
                      />
                    </a>
                  </div>
                </div>
                <div class="p-6">
                  <h4 class="font-bold text-left text-lg mb-2">
                    {item.vehicle.odom_reading}
                    {item.vehicle.odom_unit}
                  </h4>
                  <h5 class="font-bold text-left text-lg mb-3">
                    {item.vehicle.year}
                    {item.vehicle.make}
                    {item.vehicle.model}
                  </h5>
                  <h1 class="font-bold mb-3 text-xl">
                    ${item.baseValuation}
                  </h1>
                  <button
                    on:click={() => gotoDetails(item._id)}
                    data-mdb-ripple="true"
                    data-mdb-ripple-color="light"
                    class="inline-block px-6 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded-full shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out"
                    >View Details
                  </button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </section>
  {/if}
</div>
