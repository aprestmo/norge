<script>
  const fetchData = (async () => {
    const response = await fetch(
      "https://opensheet.elk.sh/1c0GORGfO4BWbS5y2lrZTmyUWMv47_ggGCG-eTKJ1GJw/Kommuner"
    );
    return await response.json();
  })();
</script>

<h2 class="text-2xl my-4">Kommuner</h2>

{#await fetchData}
  <p class="loading">Laster&hellip;</p>
{:then data}
  <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-4 flex-wrap">
    {#each data as d}
      <div class="flex items-start border-2 p-4 gap-4 rounded-md">
        <img
          class="flex-shrink-0 mt-1 w-16"
          src="https://raw.githubusercontent.com/aprestmo/norske-fylkes-kommunevaapen/master/kommuner/{d.id}.svg"
          alt={d.name_nb}
          loading="lazy"
        />
        <div class="flex flex-col">
          <h2 class="font-bold">{d.name_nb}</h2>
          <dl class="flex flex-wrap gap-1">
            <dt>Admin&shy;istra&shy;sjons&shy;senter:</dt>
            <dd>{d.adm_centre}</dd>
          </dl>
          <dl class="flex flex-wrap gap-1">
            <dt>Kommune&shy;nummer:</dt>
            <dd>{d.id}</dd>
          </dl>
          <dl class="flex flex-wrap gap-1">
            <dt>Målform:</dt>
            <dd>{d.language[0].toUpperCase() + d.language.slice(1)}</dd>
          </dl>
          {#if d.prefix === 'TRUE'}
            <a href="//www.{d.url}.kommune.no" target="_blank" rel="noreferer">www.{d.url}.kommune.no&nbsp;&rarr;</a>
          {:else}
            <a href="//{d.url}.kommune.no" target="_blank" rel="noreferer">{d.url}.kommune.no&nbsp;&rarr;</a>
          {/if}
        </div>
      </div>
    {/each}
  </div>
{:catch error}
  <p>An error occurred!</p>
{/await}

<style>
@keyframes loading {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
.loading {
  animation: 800ms linear loading alternate-reverse both infinite;
}
</style>
