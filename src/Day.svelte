<script>
    import {afterUpdate, beforeUpdate} from "svelte";
    import { Col } from "sveltestrap";
    import moment from "moment";

    let phase = "waiting";

    const phases = {
        "Full moon": "🌕",
        "New Moon": "🌑"
    }

    //TODO: implement this function and make sure it passes phase down to Day component
    let checkPhase = async(date) => {
        const URL = "http://157.245.183.160:8000/day/?date="
        let parent_response = await fetch(URL + date.format("YYYY-MM-DD[T00:00:00]"))
            .then(response => response.json())
            .then(data => phase = data)
            .finally(() => console.log("finished"))
        console.log(phase)

        console.log("Fetching from " + URL + date.toISOString())
        // return response.data
    }
    export let day;
    export let fade = true;
    $: cellClass = fade ? "active" : "inactive";
    afterUpdate( () => {
        checkPhase(moment(day));
        console.log(moment(day));
    })
</script>

<Col style="padding: 0;">
    <div class="day">
        <h4 class="{cellClass} pt-4 pb-2">{day.getDate()}</h4>
        {#if phase in phases}
            <p>{phases[phase]}</p>
        <!--{:else}-->
        <!--    <p style="display: none;">No events</p>-->
        <!--    <p>No events</p>-->
        {/if}
    </div>
</Col>

<style>
    p {
        font-size: 1rem;
    }

    h1 {
        font-size: 1.5rem;
    }

    .day {
        border-color: lightcoral;
        border-style: solid;
        min-height: 120px;
    }

    .active {

    }
    .inactive {
        color: dimgray;
    }
</style>