<script lang="ts">
  import Table from "$lib/Table.svelte";

  /** @type {import('./$types').PageData} */
  export let data;

  /** @type {import('./$types').ActionData} */
  export let form;

  let names = data.names;

  function handleEditEmail(id) {
    names = names.map(name => name.id === id ? { ...name, isEditing: true } : name);
  }

  async function handleUpdateEmail(id) {
    const name = names.find(name => name.id === id);
    const response = await fetch(`?/update`, {
      method: 'POST',
      body: new URLSearchParams({
        id: name.id,
        email: name.email,
      }),
    });

    const result = await response.json();
    if (result.success) {
      names = names.map(name => name.id === id ? { ...name, isEditing: false } : name);
    }
  }
</script>

<Table {names} on:editEmail={event => handleEditEmail(event.detail)} on:updateEmail={event => handleUpdateEmail(event.detail)} />

<div
  class="mt-10 pt-10 w-full max-w-xl p-12 mx-auto rounded-lg shadow-xl dark:bg-white/10 bg-white/30 ring-1 ring-gray-900/5 backdrop-blur-lg"
>
  <form method="POST" action="?/create">
    <div class="flex flex-wrap -mx-3 mb-2">
      <div class="w-full md:w-1/3 px-3 mb-6 md:mb-0">
        <label
          class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
          for="grid-name"
        >
          Name
        </label>
        <input
          class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
          id="name"
          type="text"
          placeholder="Enter name"
          name="name"
        />
      </div>
      <div class="w-full md:w-1/3 px-3 mb-6 md:mb-0">
        <label
          class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
          for="grid-email"
        >
          Email
        </label>
        <input
          class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
          id="email"
          type="text"
          placeholder="Enter email"
          name="email"
        />
      </div>
      <button
        type="submit"
        class="bg-yellow-500 hover:bg-blue-700 text-white font-bold mt-5 ml-2 px-2 rounded "
      >
        Create Applicant
      </button>
    </div>
  </form>

  {#if form?.success}
    <p class="pt-2">Operation Successful!</p>
  {/if}
</div>
