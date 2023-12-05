<script>
  export let data;
  const { products } = data;
  console.log(data);
</script>

{#each products as products}
  <div class="">{products.title}</div>
  <div class="">{products.description}</div>
{/each}
