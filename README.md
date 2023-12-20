Mosaic cache bug
===

## The bug

When creating a table from a parquet file using `CREATE OR REPLACE`, the data is not replaced.

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

## A weird solution

I thought the issue was due to Mosaic cacheing the results so I set the following, which fixes the issue:

```js
await vg.coordinator().configure({
  cache: false
})
```

However, it doesn't matter what the value of `cache` here is. The bug is fixed as long as this configure call is made. For example, setting it to [the defaults](https://github.com/uwdata/mosaic/blob/a3b78fef28fcc3e711bb922c97c3113aa6cf9122/docs/api/core/coordinator.md?plain=1#L43) fixes it:

```js
await vg.coordinator().configure({
  cache: true,
  index: true
})
```

This also fixes it:

```js
await vg.coordinator().configure({})
```

To reproduce this, un-comment lines 9-12 in [`src/App.svelte`](./src/App.svelte).
