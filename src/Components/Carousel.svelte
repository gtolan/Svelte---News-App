<script>
  import Carousel from "@beyonk/svelte-carousel";
  import CarouselSlide from "./shared/CarouselSlide.svelte";
  import { crossfade, scale } from "svelte/transition";
  import { ChevronLeftIcon, ChevronRightIcon } from "svelte-feather-icons";
  import { createEventDispatcher } from "svelte";

  const hasAPI = "IntersectionObserver" in window; // new

  export let irishNews;

  let loading = null;
  const [send, receive] = crossfade({
    duration: 200,
    fallback: scale
  });

  const dispatch = createEventDispatcher();

  function emitSelectedArticle(el) {
    console.log("dispatch el", el);
    dispatch("message", {
      data: el
    });
  }

  console.log(irishNews, "IN");
</script>

<style>
  .slide-content {
    min-height: 400px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-position: center;
    background-size: cover;
  }
  .news-btn {
    background: #31303087;
    height: 103%;
    width: 100%;
    min-height: 406px;
    border: none;
    font-size: 7vmin;
    color: white;
    cursor: pointer;
    margin-bottom: 0;
  }

  button.left,
  button.right {
    height: 100%;
    top: 2rem;
    color: white;
  }
</style>

<!-- //autoplay -->
<Carousel autoplay="20000" perPage={{ 800: 1, 500: 1 }}>
  <span class="control" slot="left-control">
    <ChevronLeftIcon />
  </span>
  <!-- hasAPI && i > 3 -->
  {#each irishNews as image, i}
    <CarouselSlide ind={i} {image} isLoaded={hasAPI && i < 1} />
  {/each}

  <span class="control" slot="right-control">
    <ChevronRightIcon />
  </span>
</Carousel>
