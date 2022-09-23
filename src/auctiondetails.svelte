<script>
  import { onMount } from "svelte";
  import axios from "axios";

  const params = new URLSearchParams(window.location.search);
  const auctionid = params.get("auctionid");

  let auctiondetails = {};
  let yearmakemodel = {};
  let vehimage = {};
  let spinnerLoading = false;

  let vin = {};
  let body_type = {};
  let odom = {};
  let odomunit = {};
  let trim = {};
  let engine = {};
  let transmission = {};
  let extcolor = {};

  import { createState } from "svelte-create-state";

  const [aucdetail, setAucdetail] = createState("");

  onMount(async () => {
    try {
      spinnerLoading = true;
      const response = await axios.get(
        `https://fu51ye2x3g.execute-api.us-east-1.amazonaws.com/dev/auctions/${auctionid}`,
        {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("AccessToken"),
          },
        }
      );
      spinnerLoading = false;
      setAucdetail(response.data.data.auction);
      auctiondetails = response.data.data.auction;
      yearmakemodel =
        response.data.data.auction.vehicle.year +
        " " +
        response.data.data.auction.vehicle.make +
        " " +
        response.data.data.auction.vehicle.model;
      vehimage = response.data.data.auction.vehicle.photoUrl;
      vin = response.data.data.auction.vehicle.vin;
      body_type = response.data.data.auction.vehicle.body_type;
      odom = response.data.data.auction.vehicle.odom_reading;
      odomunit = response.data.data.auction.vehicle.odom_unit;
      trim = response.data.data.auction.vehicle.trim;
      engine = response.data.data.auction.vehicle.engine_type;
      transmission = response.data.data.auction.vehicle.transmission;
      extcolor = response.data.data.auction.vehicle.ext_col;

      console.log("AuctionDetails", response.data.data);
    } catch (err) {
      console.log("Auctiondetails Err", err);
    }
  });
</script>

{#if spinnerLoading}
  <div class="flex justify-center items-center">
    <div
      class="spinner-border animate-spin inline-block w-8 h-8 md:mt-6 border-4 rounded-full"
      role="status"
    >
      <span class="visually-hidden">Loading...</span>
    </div>
  </div>
{:else}
  <nav
    class="relative w-full flex flex-wrap items-center justify-between py-4 md:my-12 bg-gray-100 text-gray-500 hover:text-gray-700 focus:text-gray-700 shadow-lg"
  >
    <div
      class="container-fluid w-full flex flex-wrap items-center justify-between px-6"
    >
      <div class="container-fluid">
        <h5 class="font-bold text-left text text-black mb-1">
          VEHICLE INFORMATION:
        </h5>
        <h5 class="font-bold text-left text text-black mb-1">
          {yearmakemodel} | {odom}
          {odomunit}
        </h5>
      </div>
    </div>
  </nav>
  <div class="container my-12 px-6 mx-auto">
    <section class="mb-32 text-gray-800 text-center md:text-left">
      <div class="block rounded-lg shadow-lg bg-white">
        <div class="flex flex-wrap items-center">
          <div
            class="grow-0 shrink-0 basis-auto block lg:flex w-full lg:w-6/12 xl:w-5/12"
          >
            <img
              src={`${vehimage}`}
              alt="Vehicle"
              class="w-full rounded-t-lg lg:rounded-tr-none lg:rounded-bl-lg"
            />
          </div>
          <div class="grow-0 shrink-0 basis-auto w-full lg:w-6/12 xl:w-7/12">
            <div class="px-6 py-12 md:px-12">
              <h2 class="text-3xl font-bold mb-6 pb-2 text-left">
                VEHICLE SPECIFICATIONS
              </h2>
              <p class="text-gray-500 mb-6 pb-2" />
              <div class="flex flex-wrap mb-6">
                <table class="table-auto w-full">
                  <tbody>
                    <tr class="justify-between">
                      <td class="text-left"
                        ><b>VIN</b><br /><span>{vin} </span></td
                      >
                      <td class="text-left"
                        ><b>BODY TYPE</b><br /><span>{body_type}</span></td
                      >
                    </tr>
                    <tr class="justify-between">
                      <td class="text-left"
                        ><b>ODOMETER</b><br /><span
                          >{odom} {odomunit}
                        </span></td
                      >
                      <td class="text-left"
                        ><b>TRIM</b><br /><span>{trim}</span></td
                      >
                    </tr>
                    <tr class="justify-between">
                      <td class="text-left"
                        ><b>YEAR, MAKE, MODEL</b><br /><span
                          >{yearmakemodel}</span
                        ></td
                      >
                      <td class="text-left"
                        ><b>ENGINE</b><br /><span>{engine}</span></td
                      >
                    </tr>
                    <tr class="justify-between">
                      <td class="text-left"
                        ><b>TRANSMISSION</b><br /><span>{transmission}</span
                        ></td
                      >
                      <td class="text-left"
                        ><b>ENGINE</b><br /><span>{extcolor}</span></td
                      >
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
{/if}
