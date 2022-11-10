<script>
  export let addToCard
  export let items
  const lastVariant = items.variants[items.variants.length - 1]
</script>

<div class="col-md-12 col-lg-4 mb-5 mb-lg-0">
  <div class="card">
    <img src={items.images[0].url} class="card-img-top" alt="Laptop" />
    <div class="card-body">
      <div class="d-flex justify-content-between">
        <p class="small">
          <a href="#!" class="text-muted">{lastVariant.title}</a>
        </p>
      </div>

      <div class="d-flex justify-content-between mb-3">
        <h5 class="mb-0">{items.title}</h5>
        <h5 class="text-dark mb-0">${lastVariant.prices[1].amount}</h5>
      </div>

      <div class="d-flex justify-content-between mb-2">
        <p class="text-muted mb-0">
          Available:
          <span class="fw-bold">{lastVariant.inventory_quantity}</span>
        </p>
        <div class="ms-auto text-warning">
          <i class="fa fa-star" />
          <i class="fa fa-star" />
          <i class="fa fa-star" />
          <i class="fa fa-star" />
          <i class="fa fa-star" />
        </div>
      </div>
      <button class="btn btn-primary" on:click={addToCard(items)}>
        Add To Cart
      </button>
    </div>
  </div>
</div>
