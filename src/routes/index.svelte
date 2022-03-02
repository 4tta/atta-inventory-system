<script>
    import supabase from '$lib/db';
    import Navbar from "$lib/Navbar.svelte"

    let date;
    let category;
    let brand;
    let description;
    let serial_number;
    let price;
    let quantity

    async function signOut() {
   	const { error } = await supabase.auth.signOut();

   	if (error) alert(error.message); // alert if error
    }

    async function saveEntry() {
        const { error } = await supabase.from('dataEntries').insert(
            {
            user_id: supabase.auth.user().id,
            date: date,
            category: category,
            brand: brand,
            description: description,
            serial_number: serial_number,
            price: price,
            quantity: quantity
            }
        );
        if (error) alert(error.message);

        location.reload(); // Refresh the page.
    }

    async function getEntries() {
   	 const { data, error } = await supabase.from('dataEntries').select();
   	 if (error) alert(error.message);

   	 return data;
    }

</script>

<nav class="navbar navbar-dark bg-secondary">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Inventory System</a>

    <form class="d-flex">
      <button class="btn btn-danger" on:click={signOut}>Logout</button>
    </form>
  </div>
</nav>



<div class="container mt-5">
  <h3 class="text-center">Computer Hardware Inventory System</h3>
    <table class="table table-bordered table-striped">
        <thead class="table-primary">
          <tr>
            <th scope="col">Date</th>
            <th scope="col">Category</th>
            <th scope="col">Brand</th>
            <th scope="col">Iteam Description</th>
            <th scope="col">Serial Number</th>
            <th scope="col">Price</th>
            <th scope="col">Quantity</th>

          </tr>
        </thead>
        <tbody>
            {#await getEntries()}
   		        <p class="text-center">Fetching data...</p>
            {:then data}
                {#each data as entry}
                    <tr>
                        <td>{entry.date}</td>
                    
                        <td>{entry.category}</td>

                        <td>{entry.brand}</td>
                    
                        <td>{entry.description}</td>

                        <td>{entry.serial_number}</td>
                    
                        <td>{entry.price}</td>
                    
                        <td>{entry.quantity}</td>
                    </tr>
                {/each}
            {:catch error}
   		 <p>Something went wrong while fetching the data:</p>
   		 <pre>{error}</pre>
   	        {/await}
          
        </tbody>
      </table>
</div>


<section class="container px-4 py-3 text-center">
    <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addData">
        New Entry
    </button>
</section>


<!-- Modal -->
<div class="modal fade" id="addData" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">New Entry</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">

        <label for="basic-url" class="form-label">Date</label>
        <div class="input-group mb-3">
            <input bind:value={date} type="date" class="form-control" >
        </div>

        <label for="basic-url" class="form-label">Category</label>
        <div class="input-group mb-3">
            <select class="form-select" bind:value={category}>
              <option selected>Choose...</option>
              <option value="PC">PC</option>
              <option value="Monitor">Monitor</option>
              <option value="Keyboard">Keyboard</option>
              <option value="Mouse">Mouse</option>
              <option value="CPU">CPU</option>
              <option value="Graphic Card">Graphic Card</option>
              <option value="Speaker">Speaker</option>
            </select>
          </div>

        <label for="basic-url" class="form-label">Brand</label>
        <div class="input-group mb-3">
            <input type="text" bind:value={brand} class="form-control" >
        </div>

        <label for="basic-url" class="form-label">Description</label>
        <div class="input-group mb-3">
            <input type="text" bind:value={description} class="form-control" >
        </div>

        <label for="basic-url" class="form-label">Serial Number</label>
        <div class="input-group mb-3">
            <input type="text" bind:value={serial_number} class="form-control" >
        </div>

        <label for="basic-url" class="form-label">Price</label>
        <div class="input-group mb-3">
            <input type="number" bind:value={price} class="form-control" >
        </div>

        <label for="basic-url" class="form-label">Quantity</label>
        <div class="input-group mb-3">
            <input type="number" bind:value={quantity} class="form-control" >
        </div>
        


      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary" on:click={saveEntry}>Save changes</button>
      </div>
    </div>
  </div>
</div>