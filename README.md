Mosaic cache bug
===

## The bug

When creating a table from a parquet file using `.loadParquet` with the `replace: true` option, the data is not replaced.

## Steps to reproduce

1. Run the mosaic server with `npm run server`
2. Run the web server with `npm run dev`
3. Click the `Load table_1` button and you should get a scatter plot
4. Click the `Load table_2` button.

*Expected result*

The chart gets reloaded with different data.

[View screencap](assets/expected.mov)

*Actual result*

The same data is loaded.

[View screencap](assets/actual.mov)

## A solution

Calling the table something different avoids the cacheing problem. The [`App.svelte`](src/App.svelte) file contains some commented out code that fixes the issue.

```js
const randomId = Math.random().toString(36).substring(2, 15);
const tableName = `table_${randomId}`;
```
