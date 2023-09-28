<script>
  import { DataTable, PaginationNav, Toolbar, ToolbarContent, Search, ToolbarSearch, Select, SelectItem, DataTableSkeleton } from "carbon-components-svelte";
  import {onMount} from "svelte";
  import './main.scss';

  /**
    * @type {any[]}
  */
  let rows= [];

  let pageSize = 25;
  let page = 1;
  /**
    * @type {any[]}
  */
  let sourceData = [];
  let totalCount = 0;
  let totalPage = 0;
  let sourceid = 0;
  let isActive = 2;
  let value = "";

  onMount(async () => {
    sourceData = await loadSourceName();
    rows = await loadData();
  });
  async function loadSourceName() {
    let result = [];
    try {
      const response = await fetch('https://webconfighq.azurewebsites.net/api/GetSourceSystems?code=1qZ3OS-Znv0D3DdpvtGzuacY3U0j7XAfDxnuT-tQa7sFAzFuo2u4HQ==');
      if (!response.ok) {
        throw new Error('Request failed');
      }
      const data = await response.json();
      for( let i = 0; i < data.Results.length; i++) {
        result.push({
          source_id: data.Results[i].source_id,
          source_name: data.Results[i].source_name
        });
      }
    } catch (error) {
      console.error(error);
    }
    return result;
  }
  async function loadData() {
    let result = [];
    try {
      let fetchUrl = 'https://webconfighq.azurewebsites.net/api/GetInputs?code=9gt1jvz6pB_fgMPHcGQbyNvQSHhyvd9wgIbFek3hQpxfAzFu5gM7ig==&';
      fetchUrl += `pagelength=${pageSize}&`;
      fetchUrl += `pageid=${page}`;
      if(sourceid > 0 ) {
        fetchUrl += `&sourceid=${sourceid}`;
      }
      if( isActive < 2 ) {
        fetchUrl += `&isactive=${isActive}`;
      }
      if(value.length > 0) {
        fetchUrl += `&search=${value}`;
      }
      const response = await fetch(fetchUrl);
      if (!response.ok) {
        throw new Error('Request failed');
      }
      const data = await response.json();
      for( let i = 0; i < data.Results.length; i++) {
        const source_name = getSourceNameById(data.Results[i].source_id);
        result.push({
          id: data.Results[i].id,
          table_name: data.Results[i].table_name,
          schema: data.Results[i].table_schema,
          database_name: data.Results[i].database_name,
          source_system: source_name,
          source_id: data.Results[i].source_id,
          active: data.Results[i].is_active ? 'Y' : 'N'
        });
      }
      if( data.Results.length == 0 ) {
        result.push({
          id: "",
          table_name: "",
          schema: "",
          database_name: "",
          source_system: "",
          source_id: "",
          active: ""
        });
      }
      totalCount = data.Count;
      totalPage = data.PageCount;
    } catch (error) {
      console.error(error);
    }
    return result;
  }
  /**
    * @param {number} source_id
  */
  function getSourceNameById(source_id) {
    for( let i = 0; i < sourceData.length;i++) {
      if(sourceData[i].source_id == source_id) return sourceData[i].source_name;
    }
    return '';
  }
  /**
    * @param {number | any} value
  */
  async function changePageSize(value) {
    rows = [];
    pageSize = value;
    page = 1;
    rows = await loadData();
  }
  async function changePage() {
    rows = [];
    rows = await loadData();
  }
  /**
    * @param {string | number} value
  */
  async function changeSystemOpt(value) {
    rows = [];
    sourceid = getSourceIdByName(value);
    rows = await loadData();
  }
  /**
    * @param {string | number} value
  */
  async function changeActiveOpt(value) {
    rows = [];
    isActive = value == 'Y' ? 1 : 0;
    if( value == "Active") isActive = 2;
    rows = await loadData();
  }
  /**
    * @param {KeyboardEvent} e
  */
  async function searchText(e) {
    if(e.key != "Enter") return;
    rows = [];
    rows = await loadData();
  }
  async function searchFullText() {
    value = "";
    rows = [];
    rows = await loadData();
  }
  /**
     * @param {string | number} name
  */
  function getSourceIdByName(name) {
    for( let i = 0; i < sourceData.length;i++) {
      if(sourceData[i].source_name == name) return sourceData[i].source_id;
    }
    return 0;
  }
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<section class="py-4">
  <h4 class="w-fit pt-6 mb-5">Table List</h4>
  {#if rows.length < 1}
    <DataTableSkeleton />
  {:else}
    <DataTable
      headers={[
        { key: "id", value: "id" },
        { key: "table_name", value: "table name" },
        { key: "schema", value: "schema" },
        { key: "database_name", value: "database name" },
        { key: "source_system", value: "source system" },
        { key: "active", value: "active" }
      ]}

      {pageSize}
      {rows}
    >
            <div class="flex justify-between w-full mb-8">
              <div class="flex">
                <Search
                  bind:value
                  class="!w-[325px]"
                  on:keydown={async (e)=> searchText(e)}
                  on:click={async (e)=> searchFullText()}
                />
                <div>
                  <Select 
                    inline
                    selected = {getSourceNameById(sourceid)}
                    on:update={async (e)=> changeSystemOpt(e.detail)}
                  >
                    <SelectItem value="Source System" />
                    {#each sourceData as data, index}
                      <SelectItem value={data.source_name} />
                    {/each}
                  </Select>
                </div>
                <div>
                  <Select 
                    inline
                    selected={isActive == 2 ? "Active" : isActive == 1? "Y" : "N"}
                    on:update={async (e)=> changeActiveOpt(e.detail)}
                  >
                    <SelectItem value="Active" />
                    <SelectItem value="Y" />
                    <SelectItem value="N" />
                  </Select>
                </div>
              </div>
              
              <div class="flex items-center">
                <span class="mr-2">Showing</span>
                <Select 
                  inline 
                  size="sm" 
                  class="mr-4"
                  selected={pageSize}
                  on:update={async (e)=> changePageSize(e.detail)}
                >
                  <SelectItem value={25}/>
                  <SelectItem value={50}/>
                  <SelectItem value={75}/>
                  <SelectItem value={100}/>
                </Select>
                <span>of {totalCount} results</span>
              </div>
            </div>
    </DataTable>
    <PaginationNav
      class="flex justify-end items-end mt-3"
      shown={3} 
      total={totalPage} 
      bind:page 
      on:change={async (e) => changePage()}
    />
  {/if}
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

</style>
