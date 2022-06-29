<script>
    import {loading, books} from '../store' 
    let text = '';
    let alertMessage = '';
    let searchInput;

    function handleSubmit(event){
        event.preventDefault();
        if (!text){
            setAlert('Por favor ingrese texto')
        }else{
            searchBooks();
        }
    }

    function limpiarBusqueda(){
        text = '';
        books.set([]);
        searchInput.focus();
    }
    
    async function searchBooks(){
        loading.set(true);
        const res = await fetch('https://www.googleapis.com/books/v1/volumes?q=${text}');
        const data = res.json();
        books.set(data.items);
        loading.set(false);
    }

    function setAlert(msg){
        alertMessage = msg;
        setTimeout(() => alertMessage = '', 1000)
    }

</script>

{#if alertMessage}
    <div class="alert">
        <h3>
            {alertMessage}
        </h3>
    </div>
{/if}

<div class="container mx-auto">
    <div class="BarradeNavegacion">
        <from on:submit={handleSubmit}>
            <div class="pb-3">
                <input class="w-50 p-3 rounded" 
                    type="text" 
                    placeholder="Ingrese el titulo del autor o la categoria" 
                    bind:value={text}
                    bind:this={searchInput}   
                >
            </div>
            <div>
                <button type="submit" class="btn btn-primary w-50 p-3 rounded">Buscar Libros</button>
            </div>
        </from>
        {#if books.length > 0 }
            <button class="Limpiar" on:click={limpiarBusqueda}>Limpiar Busqueda</button>
        {/if}
</div>
</div>
