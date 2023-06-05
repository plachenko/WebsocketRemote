<script>
    import { createEventDispatcher } from 'svelte';
  
    let isPointerDown = false;
    let markerDiv;
  
    const dispatch = createEventDispatcher();
  
    function handlePointerDown(event) {
      isPointerDown = true;
      console.log(markerDiv);
      markerDiv.style.display = "block";
      markerDiv.style.left = event.clientX - 50 + 'px';
      markerDiv.style.top = event.clientY - 50 + 'px';
      console.log(event);
      dispatch('pointerdown', {event});
    }
  
    function handlePointerUp(event) {
      markerDiv.style.display = "none";
      isPointerDown = false;
      dispatch('pointerdown', {event});
    }
</script>
<div
    class="pointer-container"
    on:pointerdown={handlePointerDown}
    on:pointerup={handlePointerUp}
>
    <div bind:this={markerDiv} id="marker"></div>
</div>


<style>
    #marker {
        position: absolute;
        width: 100px;
        height: 100px;
        background-color: #F00;
        display:none;
        border-radius: 100px;
        }

    .pointer-container {
      width: 100%;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 24px;
      position: absolute;
    }
</style>