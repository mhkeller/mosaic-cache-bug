<script>
  import * as vg from '@uwdata/vgplot';

  let chartElement;

  async function loadTable(fileName) {
    await vg.coordinator().clear({ clients: false, cache: true });

    // A random table name will show fresh data
    const randomId = Math.random().toString(36).substring(2, 15);
    const tableName = `table_${randomId}`;

    // Reusing the same namespace will show cached data
    // const tableName = 'table_1';

    await vg.coordinator().exec(
      vg.loadParquet(
        tableName,
        `http://localhost:5173/data/${fileName}.parquet`,
        {
          replace: true,
        }
      )
    );

    const chart = vg.plot(
      vg.dot(vg.from(tableName), {
        x: 'sepal_length',
        y: 'sepal_width',
        fill: 'species',
        r: 5.5,
        stroke: 'white',
        strokeWidth: 1,
        tip: true,
      })
    );

    chartElement.innerHTML = '';
    chartElement.appendChild(chart);
  }
</script>

<button on:click={() => loadTable('full_table')}>Load table_1</button>
<button on:click={() => loadTable('filtered_table')}>Load table_2</button>
<div class="container">
  <div bind:this={chartElement} />
</div>
