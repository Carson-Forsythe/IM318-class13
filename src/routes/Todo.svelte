<script>
  import { onMount } from "svelte";


let todoItem = $state('');
let todoList = $state([]);
let doneList = $state([]);
let storedList;

onMount(() => {
     storedList = localstorage.getItem('storedList');
     if (storedList) {
          todoList = (JSON.parse(storedList));
     }
})
function updateList(){
     return storedList = localStorage.setItem('storedItem', JSON.stringify (todoList));
}


function addItem(event){
     event.preventDefault();
     if (todoItem ==''){
          return;
     }
     todoList = [...todoList,{
          text: todoItem,
          done: false
     
     }];
    
     updateList();
     todoItem= ''; 
}
function removeItem(index){
     todoList = todoList.toSpliced(index, 1);
     updateList();
}
function nuke(){
     todoList = [];
}

$inspect(todoList);

</script>
<form onsubmit={addItem}>
<input type="text" bind:value={todoItem}>
<button type="submit">Add</button>
</form>

<ul>
     {#each todoList as item, index}
          <li> <input type="checkbox" bind:checked={item.done}>
          <span class:done={item.done}>{item.text}</span>
          <button type="button" onclick={() => removeItem(index)}>X</button>
          

     </li>
     {/each}
</ul>
{#if (todoList.length > 0)}
<button type="button" onclick={nuke}> Erase</button>
{/if}

<style>
     input[type="text"]{
          font-size: 105%;
     }
     button{
          font-size: 105%;
     }
     ul{
          list-style: none;
          text-align: center;
     }
span.done{
     color: crimson;
     text-decoration: line-through;
}
</style>