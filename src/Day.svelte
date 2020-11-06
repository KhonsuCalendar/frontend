<script>
    let phase = "Full";
    export let fade = true;
    export let day = 0;
    $: cellClass = fade ? "active" : "inactive";
</script>

<h1 class="{cellClass}">{day}</h1>
<p>{phase}</p>

<style>
    .active {

    }
    .inactive {
        color: dimgray;
    }
</style>