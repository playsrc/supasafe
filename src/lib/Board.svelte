<script lang="ts">
  let listItems = [1];

  function addMore() {
    listItems.push(listItems.length + 1);
    listItems = listItems;
  }

  function deleteOne() {
    if (listItems.length === 1) return;
    listItems.pop();
    listItems = listItems;
  }
</script>

<div class="container">
  <div class="mt-5 flex gap-2 items-center">
    <p>If</p>
    <input class="w-28" type="text" placeholder="type here..." />
    <p>were to be</p>
    <input class="w-28" type="text" placeholder="type here..." />
    <p>I'd need to:</p>
  </div>

  <ul class="ml-5 mt-5 list-decimal flex flex-col gap-2">
    {#each listItems as _}
      <li>
        <input class="w-full" type="text" placeholder="type here..." />
      </li>
    {/each}
    <li>
      <button on:click={addMore}> 🌱 Add more</button>
      <button on:click={deleteOne}>🔥 Delete one</button>
    </li>
  </ul>
</div>
