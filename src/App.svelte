<script>
  import { crossfade, scale } from "svelte/transition";
  import images from "./images.js";
  import Navbar from "./Components/Navbar.svelte";
  import Carousel from "./Components/Carousel.svelte";
  //   import GlobalStyles from "./Styles/global.css";

  let articles = [];
  let articleImages = [];
  let promise = getLatestNews();
  let text;

  let articlesIreland = [];
  let articlesIrelandImages = [];
  let promise2 = getLatestNewsIreland();
  async function getLatestNews() {
    const res = await fetch(`https://newsapi.org/v2/top-headlines?country=us`, {
      headers: {
        "X-Api-Key": "2781df46bd1641a8bf05d22a40ddd0e9"
      }
    });
    text = await res.json();
    articles = text.articles;
    articleImages = articles.map(article => article.urlToImage);

    console.log(text, "text");
    console.log(articleImages, "articleImages");
    if (res.ok) {
      return text;
    } else {
      throw new Error(text);
    }
  }

  async function getLatestNewsIreland() {
    const res = await fetch(`https://newsapi.org/v2/top-headlines?country=ie`, {
      headers: {
        "X-Api-Key": "2781df46bd1641a8bf05d22a40ddd0e9"
      }
    });
    text = await res.json();
    articlesIreland = text.articles;
    articlesIrelandImages = articlesIreland.map(
      articlesIreland => articlesIreland.urlToImage
    );

    console.log(text, "text");
    console.log(articlesIrelandImages, "articleImages");
    if (res.ok) {
      return text;
    } else {
      throw new Error(text);
    }
  }

  const [send, receive] = crossfade({
    duration: 200,
    fallback: scale
  });

  let selected = null;
  let loading = null;

  //   const ASSETS = `https://sveltejs.github.io/assets/crossfade`;

  const load = image => {
    console.log("image", image);
    const timeout = setTimeout(() => (loading = image), 100);

    const img = new Image();

    img.onload = () => {
      selected = image;
      clearTimeout(timeout);
      loading = null;
    };

    img.src = image.urlToImage;
  };

  function handleMessageFromCarousel(event) {
    console.log("app.sv", event.detail.data);
    load(event.detail.data);
  }

  const handleNavQuery = event => {
    console.log(event.detail.data, "NAV Q in APP");
  };
</script>

<style>
  :global(body) {
    padding: 0;
  }
  .container {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
  }

  .phone {
    position: relative;
    display: flex;
    flex-direction: column;
    width: 100vw;
    height: 100vh;

    border-bottom-width: 10vmin;
    padding: 3vmin;
    border-radius: 2vmin;
  }

  h1 {
    font-weight: 300;
    text-transform: uppercase;
    font-size: 5vmin;
    margin: 0.2em 0 0.5em 0;
  }

  button {
    width: 100%;
    height: 100%;
    color: white;
    font-size: 4vmin;
    border: none;
    margin: 0;
    will-change: transform;
    min-height: 280px;
    margin-top: 10px;
    padding: 3rem;
  }

  .photo,
  img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
  }

  .photo {
    display: flex;
    align-items: stretch;
    justify-content: flex-end;
    flex-direction: column;
    will-change: transform;
  }

  img {
    object-fit: cover;
    cursor: pointer;
  }

  .credit {
    text-align: right;
    font-size: 2.5vmin;
    padding: 1em;
    margin: 0;
    color: white;
    font-weight: bold;
    opacity: 0.6;
    background: rgba(0, 0, 0, 0.4);
  }

  .credit a,
  .credit a:visited {
    color: white;
  }
  .max-size {
    max-height: 5rem;
    max-width: 5rem;
  }
  p.text {
    height: 44vh;
    background: white;
  }
  img.main-image {
    max-height: 32vh;
    position: relative;
  }
  .photo.container {
    position: absolute;
    top: 0;
    left: 2.5%;
    width: 95%;
    height: 100%;
    overflow: hidden;
    background: white;
    margin-top: 5rem;
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0px 0px 2px 2px #00000099;
    justify-content: flex-start;
    flex-direction: column;
  }
  .news-btn {
    background: #6558588c;
    cursor: pointer;
    margin: 0;
    min-height: 300px;
    transition: 0.3s ease-in-out;
  }
  .news-btn:hover {
    background: transparent;
    cursor: pointer;
    font-size: 1rem;
  }
  .square {
    background-position: center;
    background-size: cover;
    min-height: 300px;
  }
  .container .phone div.card {
    top: 0;
    position: fixed;
    width: 100vw;
    z-index: 51;
    display: block;
    height: 100vh;
    left: 0;
    background: white;
  }

  .grid {
    display: grid;
    flex: 1;
    grid-template-columns: repeat(1, 1fr);
    grid-template-rows: repeat(4, 1fr);
    grid-gap: 2vmin;
  }
  .carousel .slides {
    min-height: 300px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  @media only screen and (min-width: 600px) {
    .grid {
      display: grid;
      flex: 1;
      grid-template-columns: repeat(2, 1fr);
      grid-template-rows: repeat(4, 1fr);
      grid-gap: 2vmin;
    }
  }
  @media only screen and (min-width: 900px) {
    .grid {
      display: grid;
      flex: 1;
      grid-template-columns: repeat(3, 1fr);
      grid-template-rows: repeat(4, 1fr);
      grid-gap: 2vmin;
    }
  }
</style>

<main>
  <Navbar on:inputSearchClicked={handleNavQuery} />
  {#await promise2}
    <p>...waiting</p>
  {:then}
    <Carousel
      on:message={handleMessageFromCarousel}
      irishNews={articlesIreland}
      selected />
  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}

  <div class="container">
    <div class="phone">
      <div class="grid">
        {#each articles as image}
          <div class="square" style="background-image: url({image.urlToImage})">
            {#if selected !== image}
              <button
                class="news-btn"
                on:click={() => load(image)}
                in:receive={{ key: image.title }}
                out:send={{ key: image.title }}>
                {loading === image.urlToImage ? '...' : image.title}
              </button>
            {/if}
          </div>
        {/each}
      </div>

      {#if selected}
        {#await selected then d}
          <div
            class="card"
            in:receive={{ key: d.title }}
            out:send={{ key: d.title }}>
            <div on:click={() => (selected = null)} class="photo container">

              <img alt={d.author} src={d.urlToImage} class="main-image" />
              <h1>{d.title}</h1>
              <p class="text">{d.content}</p>

            </div>
          </div>
        {/await}
      {/if}
    </div>
  </div>
</main>
