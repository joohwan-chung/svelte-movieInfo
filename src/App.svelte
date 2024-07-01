<script>
  import data from './lib/movies';
  import Navbar from './lib/components/Navbar.svelte';
  import Modal from './lib/components/Modal.svelte';
  import Movies from './lib/components/Movies.svelte';
  import Event from './lib/components/Event.svelte';
  import SearchBar from './lib/components/SearchBar.svelte';

  let data_temp = [...data];

  let likeCount = 0;
  
  const handleLike = (i) => {
    data[i].likeCount += 1;
    console.log(likeCount, i)
  }

  let isModal = false;
  let selectedMovie = 0;

  const closeModal = () => {
    isModal = false;
  }

  const handleMovieNumber = (i) => {
    selectedMovie = i;
  }

  let isEvent = true; // 이벤트창 표시 여부
</script>

<Navbar />

<!-- <div class={isEvent ? 'event show' : 'event'}> -->
{#if isEvent}
  <Event bind:isEvent />
{/if}

<SearchBar {data} bind:data_temp />

<Movies
  {data_temp}
  {data}
  bind:isModal
  {handleMovieNumber}
  {handleLike}
/>

{#if isModal}
  <Modal 
    {data}
    {selectedMovie}
    {closeModal}
  />
{/if}

<style>
</style>