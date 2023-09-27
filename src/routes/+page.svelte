<script>
  import { DataTable, Pagination, Search } from "carbon-components-svelte";
  import {onMount} from "svelte";


  let rows = [
    {
      id: 14,
      table_name: "ANIMASTER",
      schema: "landing-zone-pr",
      database_name: "ETL",
      source_system: "PR",
      active: "Y"
    },
    {
      id: 193,
      table_name: "BIAST ASSETNUMSYS",
      schema: "landing-zone-ces",
      database_name: "ETL",
      source_system: "CES",
      active: "Y"
    },
    {
      id: 194,
      table_name: "BiPRK FP RATING",
      schema: "landing-zone-ces",
      database_name: "ETL",
      source_system: "PR",
      active: "Y"
    },
    {
      id: 523,
      table_name: "CBPOR ENTITY LINK",
      schema: "landing-zone-ces",
      database_name: "ETL",
      source_system: "PR",
      active: "N"
    },
    {
      id: 279,
      table_name: "CES BIAST ASSETNUMSYS",
      schema: "dlt",
      database_name: "ETL",
      source_system: "CES",
      active: "N"
    },
    {
      id: 236,
      table_name: "CES BIAST ASSETNUMSYS",
      schema: "cmp",
      database_name: "ETL",
      source_system: "CES",
      active: "Y"
    },
    {
      id: 633,
      table_name: "ANIMASTER",
      schema: "landing-zone-pr",
      database_name: "ETL",
      source_system: "PR",
      active: "Y"
    },
    {
      id: 721,
      table_name: "ANIMASTER",
      schema: "cmp",
      database_name: "ETL",
      source_system: "PR",
      active: "N"
    },
    {
      id: 543,
      table_name: "ANIMASTER",
      schema: "landing-zone-pr",
      database_name: "ETL",
      source_system: "PR",
      active: "Y"
    },
    {
      id: 111,
      table_name: "ANIMASTER",
      schema: "landing-zone-pr",
      database_name: "ETL",
      source_system: "CES",
      active: "Y"
    },
    {
      id: 114,
      table_name: "ANIMASTER",
      schema: "landing-zone-pr",
      database_name: "ETL",
      source_system: "PR",
      active: "Y"
    },
    {
      id: 134,
      table_name: "ANIMASTER",
      schema: "landing-zone-pr",
      database_name: "ETL",
      source_system: "PR",
      active: "Y"
    },

    {
      id: 154,
      table_name: "ANIMASTER",
      schema: "landing-zone-pr",
      database_name: "ETL",
      source_system: "PR",
      active: "Y"
    },
    {
      id: 164,
      table_name: "ANIMASTER",
      schema: "landing-zone-pr",
      database_name: "ETL",
      source_system: "PR",
      active: "Y"
    }
  ];

  let pageSize = 15;
  let page = 1;
  onMount(() => {
    loadData();
  });
  async function loadData() {
    try {
      const response = await fetch('https://webconfighq.azurewebsites.net/api/GetInputs?code=9gt1jvz6pB_fgMPHcGQbyNvQSHhyvd9wgIbFek3hQpxfAzFu5gM7ig==', {
        headers: {
          'Content-Type': 'application/json',
          'X-Atlassian-Token': 'no-check',
        },
      });
      if (!response.ok) {
        throw new Error('Request failed');
      }
      const data = await response.json();
      console.log("data=",data)
    } catch (error) {
      console.error(error);
    }
  }
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
  <h4 class="w-fit pt-6 mb-5">Table List</h4>
  <div class="mb-5 flex justify-between">
    <div class="flex">
      <Search />
      <select
        name="source_system"
        id="source_system"
        class="rounded ml-3 border-2 border-[#ecedee] bg-[#f4f4f4]"
      >
        <option value="blank">Source System</option>
        <option value="PR">PR</option>
        <option value="CES">CES</option>
      </select>
      <select
        name="schema"
        id="schema"
        class="rounded ml-3 border-2 border-[#ecedee] bg-[#f4f4f4]"
      >
        <option value="blank">Schema</option>
        <option value="PR">PR</option>
        <option value="CES">CES</option>
      </select>
      <select
        name="active"
        id="active"
        class="rounded ml-3 border-2 border-[#ecedee] bg-[#f4f4f4]"
        value="CES"
      >
        <option value="blank">Active</option>
        <option value="PR">PR</option>
        <option value="CES">CES</option>
      </select>
    </div>
    <div class="flex">
      <p>showing</p>
      <select
        name="page_amount"
        id="page_amount"
        class="rounded ml-3 border-2 border-[#ecedee] bg-[#f4f4f4] h-fit text-sm py-0 pr-2"
        bind:value={pageSize}
      >
        <option value={5}>5</option>
        <option value={10}>10</option>
        <option value={15}>15</option>
      </select>
      <p>of 60 results</p>
    </div>
  </div>

  <DataTable
    sortable
    class="w-full"
    headers={[
      { key: "id", value: "id" },
      { key: "table_name", value: "table name" },
      { key: "schema", value: "schema" },
      { key: "database_name", value: "database name" },
      { key: "source_system", value: "source system" },
      { key: "active", value: "active" }
    ]}
    {pageSize}
    {page}
    {rows}
  />
</section>

<style>
  section {
    padding-left: 35px;
    padding-right: 35px;
    margin-left: 20px;
    margin-right: 20px;
    background-color: white;
    border-radius: 10px;
    box-shadow: 3px 3px 3px rgb(0, 0, 0, 10%) inset;
  }

  :global(
      .bx--data-table--sort th:first-of-type,
      .bx--data-table th:first-of-type[aria-sort]
    ) {
    border-top-left-radius: 6px;
  }

  :global(.bx--table-sort:first-of-type) {
    border-top-left-radius: 6px;
  }

  :global(
      .bx--data-table--sort th:last-of-type,
      .bx--data-table th:last-of-type[aria-sort]
    ) {
    border-top-right-radius: 6px;
  }

  :global(.bx--table-sort:last-of-type) {
    border-top-right-radius: 6px;
  }

  :global(.bx--data-table td, .bx--data-table tbody th) {
    background-color: white;
  }

  /* Search Button Style */

  :global(.bx--search) {
    width: 60%;
  }

  :global(
      .bx--search--xl .bx--search-input,
      .bx--search--xl.bx--search--expandable.bx--search--expanded
        .bx--search-input
    ) {
    border-radius: 4px;
    border: 2px solid #ecedee;
  }
</style>
