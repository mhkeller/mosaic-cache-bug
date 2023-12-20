<script>
  import { onMount } from "svelte";

  import * as vg from "@uwdata/vgplot";

  let chartElement;

  async function loadTable(fileName) {
    await vg.coordinator().configure({
      cache: true,
      index: true
    })
    await vg.coordinator().exec(
      `CREATE OR REPLACE TABLE table_1 AS SELECT * FROM 'http://localhost:5173/data/${fileName}.parquet'`
    );

    const chart = vg.plot(
      vg.dot(vg.from('table_1'), {
        x: 'sepal_length',
        y: 'sepal_width',
        fill: 'species',
        r: 5.5,
        stroke: 'white',
        strokeWidth: 1,
        tip: true,
      }),
    );

    chartElement.innerHTML = '';
    chartElement.appendChild(chart);
  };

</script>

<button on:click={() => loadTable('full_table')}>Load table_1</button>
<button on:click={() => loadTable('filtered_table')}>Load table_2</button>
<div class="container">
  <div bind:this={chartElement} />
</div>
