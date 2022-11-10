<script>
  import ProductList from './Product-list.svelte'
  import { onMount } from 'svelte'
  import { writable, derived } from 'svelte/store'

  const apiData = writable([])
  onMount(async () => {
    async function getData() {
      try {
        const response = await fetch('http://localhost:9000/store/products/', {
          method: 'GET',
          headers: {
            accept: 'text/html',
            'Content-Type': 'text/html',
          },
        })

        if (!response.ok) {
          throw new Error(`Error! status: ${response.status}`)
        }

        const result = await response.json()
        apiData.set(result)
        return result
      } catch (err) {
        console.log(err)
      }
    }
    getData()
  })
  export const products = derived(apiData, ($apiData) => {
    if ($apiData.products) {
      return $apiData.products.map((drink) => drink)
    }
    return []
   })

  let cartItems = []

  const addToCard = async (item) => {
    const itemIndex = cartItems.findIndex((obj) => obj.item.id == item.id)
    //if item is not already in cart
    if (itemIndex === -1) {
      //add new item to card
      cartItems = [...cartItems, { item, quantity: 1 }]
    } else {
      //Increase the quantity of item
      cartItems[itemIndex].quantity += 1
    }
  }
  $: cartTotal = cartItems.reduce((sum, itemData) => {
    return (
      sum +
      itemData.item.variants[itemData.item.variants.length - 1].prices[1]
        .amount *
        itemData.quantity
    )
  }, 0)
</script>
<nav class="navbar navbar-light bg-light mb-4">
  <div class="container-fluid">
    <span class="navbar-brand ">Svelte Store</span>
    <form class="d-flex">
      <input
        class="form-control me-2"
        type="search"
        placeholder="Search"
        aria-label="Search" />
      <button class="btn btn-outline-success" type="submit">Search</button>
    </form>
    <p
      data-bs-toggle="offcanvas"
      data-bs-target="#offcanvasRight"
      aria-controls="offcanvasRight"
      style="cursor: pointer">
      Cart
      <i class="fa-sharp fa-solid fa-cart-shopping" />
      <span class="badge bg-secondary">{cartItems.length}</span>
    </p>
  </div>
</nav>
<div
  class="offcanvas offcanvas-end"
  tabindex="-1"
  id="offcanvasRight"
  aria-labelledby="offcanvasRightLabel">
  <div class="offcanvas-header">
    <h5 id="offcanvasRightLabel">Cart Items</h5>
    <button
      type="button"
      class="btn-close text-reset"
      data-bs-dismiss="offcanvas"
      aria-label="Close" />
  </div>
  <div class="offcanvas-body">
    <ol class="list-group list-group-numbered">
      {#each cartItems as cart}
        <li
          class="list-group-item d-flex justify-content-between
          align-items-start">
          <div class="ms-2 me-auto">
            <div class="fw-bold">{cart.item.title}</div>
          </div>
          <span class="badge bg-primary rounded-pill">{cart.quantity}</span>
        </li>
      {/each}
    </ol>
    <div class="d-flex justify-content-between mt-4">
      <p>Cart Total:</p>
      <p>${cartTotal}</p>
    </div>
    <div class="d-grid gap-2">
      <button class="btn btn-primary" type="button">Checkout</button>
    </div>
  </div>
</div>
<section style="background-color: #eee;">
  <div class="container py-5">
    <div class="row">
      {#each $products as product}
        <ProductList {addToCard} items={product} />
      {/each}
    </div>
  </div>
</section>