<script>
  import { onMount, onDestroy } from 'svelte';
  import data from './lib/movies';
  import Navbar from './lib/components/Navbar.svelte';
  import Modal from './lib/components/Modal.svelte';
  import Movies from './lib/components/Movies.svelte';
  import Event from './lib/components/Event.svelte';
  import SearchBar from './lib/components/SearchBar.svelte';

  const eventText = [
    "영화 정보 업데이트",
    "신규 영화 추가",
    "이벤트 진행중"
  ]

  let data_temp = [...data];

  let likeCount = 0;
  
  const handleLike = (id) => {
    // data[i].likeCount += 1;
    // console.log(likeCount, i)
    data.map(movie => {
      if (movie.id === id) {
        movie.likeCount += 1;
      }
    });
    // data_temp = [...data];
    // data_temp 에 있는 내용만 필터링해서 복사
    data_temp = data.filter(movie => {
      return data_temp.includes(movie);
    });
  }

  let isModal = false;
  let selectedMovie = 0;

  const closeModal = () => {
    isModal = false;
  }

  const handleMovieNumber = (i) => {
    selectedMovie = i;
  }

  let alertText = "";

  let isEvent = true; // 이벤트창 표시 여부
  let eventIndex = 0; // 이벤트 텍스트 인덱스
  let intervalEventText;

  $: {
    // 이벤트 인터벌 제거
    clearInterval(intervalEventText);

    // 일정 시간 경과 후 eventIndex를 1증가
    intervalEventText = setInterval(() => {
      eventIndex += 1;
      if (eventIndex >= eventText.length) {
        eventIndex = 0;
      }
    }, 3000);
  }
</script>

<Navbar />

<!-- <div class={isEvent ? 'event show' : 'event'}> -->
{#if isEvent}
  <Event bind:isEvent {eventText} {eventIndex} />
{/if}

<SearchBar {data} bind:data_temp bind:alertText />

<!-- 전체보기 버튼 -->
<div class="container">
  <button on:click={() => {
    data_temp = [...data];
    alertText = "";
  }}>전체보기
  </button>
</div>

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
  .container {
    text-align: center;
  }
</style>