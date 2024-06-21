<script>
	import { onMount } from 'svelte';
	import { dogImages } from '../store';
    

	let error = null;

	let page = 1;

	async function loadImages() {
    try {
      const res = await fetch(`https://dog.ceo/api/breeds/image/random/12?page=${page}`);
      const data = await res.json();
      dogImages.set(data.message);
    } catch (err) {
      error = err;
    }
  }

	function removeImage(index) {
		dogImages.update((images) => {
			images.splice(index, 1);
			return images;
		});
	}

	function nextPage() {
		page += 1;
		loadImages();
	}

	function prevPage() {
		if (page > 1) {
			page -= 1;
			loadImages();
		}
	}

    onMount(loadImages);
</script>

<main class="min-h-screen p-4 bg-gradient-to-br from-blue-100 to-purple-200">
    <h1 class="text-4xl font-bold text-center mb-4">Dog Images</h1>
    {#if error}
        <p class="text-red-600 text-center">Error loading images: {error.message}</p>
    {:else}
        <div class="container mx-auto px-5 py-2 lg:px-32 lg:pt-12">
            <div class="-m-1 flex flex-wrap md:-m-2">
                {#each $dogImages as image, index}
                    <div class="flex w-full sm:w-1/2 md:w-1/3 lg:w-1/4 p-1 md:p-2">
                        <div class="relative group w-full">
                            <img src={image} alt="Dog" class="block h-64 w-full rounded-lg transition duration-300 ease-in-out group-hover:scale-105 shadow-2xl" />
                            <button
                                class="absolute top-1 right-3 text-white rounded-full p-1 text-2xl transition duration-300 ease-in-out group-hover:scale-125"
                                on:click={() => removeImage(index)}>
                                &times;
                            </button>
                        </div>
                    </div>
                {/each}
            </div>
        </div>
        <div class="flex justify-center mt-4">
            <button class="bg-gray-500 text-white px-4 py-2 mr-4 rounded disabled:bg-gray-300 hover:bg-gray-600 disabled:hover:bg-gray-300" on:click={prevPage} disabled={page === 1}>Anterior</button>
            <button class="bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded" on:click={nextPage}>Siguiente</button>
        </div>
    {/if}
</main>
  <style>

  </style>
