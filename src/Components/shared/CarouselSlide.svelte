<script>
  console.log("car slide start");
  import { crossfade, scale } from "svelte/transition";
  export let image;

  let observer = null;
  export let isLoaded = false;
  if (!isLoaded) {
    observer = new IntersectionObserver(onIntersect, { rootMargin: "10px" });
  }

  function onIntersect(entries) {
    if (!isLoaded && entries[0].isIntersecting) {
      isLoaded = true;
      console.log("load it now!");
    }
  }

  function lazyLoad(node) {
    observer && observer.observe(node);
    console.log("observing", node);
    return {
      destroy() {
        observer && observer.unobserve(node);
      }
    };
  }

  import { createEventDispatcher } from "svelte";

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
  console.log("car slide end");
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
</style>

<div
  use:lazyLoad
  class="slide-content"
  style="background-image: url({isLoaded ? image.urlToImage : ''})">
  <button class="news-btn" on:click={emitSelectedArticle(image)}>
    {image.title}
  </button>
</div>
