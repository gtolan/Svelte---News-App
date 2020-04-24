<script>
  import Button from "./shared/Button.svelte";
  import { createEventDispatcher } from "svelte";

  const materialBtn = {
    fontSize: ".875rem",
    width: 100,
    height: 42,
    color: "#fff",
    bgColor: "98, 0, 238",
    rippleColor: "#c199f9",
    rippleBlur: 0,
    round: ".2rem",
    opacityIn: 0.5,
    shadow: 4,
    shadowHover: 5,
    shadowActive: 2,
    speed: 900
  };

  const dispatch = createEventDispatcher();
  let searchQuery = "";

  function emitSearchQuery(query) {
    console.log("dispatch el", query);
    dispatch("inputSearchClicked", {
      data: query
    });
  }

  const handleQuery = () => {
    if (searchQuery !== "" && searchQuery.length > 3) {
      emitSearchQuery(searchQuery);
    }
  };
</script>

<style>
  nav {
    height: 3.5rem;
    background-color: #21af7a;
    display: flex;
    flex-direction: row;
    text-align: center;
    justify-content: center;
    width: 100vw;
    color: white;
    z-index: 52;
    position: fixed;
    box-shadow: 0px 0px 2px 2px #000000ad;
    font-size: 3vmin;
  }
  nav .search-container {
    position: absolute;
    right: 0;
    margin-top: 0.55rem;
    margin-right: 1rem;
    background: white;
    height: 2.45rem;
    overflow: hidden;
    display: flex;
    border-radius: 30px;
  }
  nav .search-container input {
    outline: none;
    height: 100%;
    margin: 0;
    vertical-align: text-bottom;
    padding: 0;
    padding-left: 10px;
  }
</style>

<nav>
  <p>Irish News</p>
  <div class="search-container">
    <input bind:value={searchQuery} placeholder="query news stories" />
    <Button on:click={handleQuery} {...materialBtn}>Search</Button>
  </div>
</nav>
