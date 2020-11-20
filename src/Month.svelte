<script>
    import { onMount } from 'svelte';
    import Day from './Day.svelte';
    import { Col, Row, Button } from 'sveltestrap';

    let today = new Date();
    export let currentMonth = today.getMonth();
    export let currentYear = today.getFullYear();
    let weeksOfMonth = [];
    let firstOfMonth;

    let buildMonth = (day, month, year) => {
        weeksOfMonth = [];
        let initial_day = new Date(year, month, day);
        initial_day.setDate(initial_day.getDate() - initial_day.getDay());

        for (let week = 0; week < 6; week++){
            let daysOfWeek = [];
            for (let day = 0; day < 7; day++){
                daysOfWeek.push(new Date(initial_day));
                initial_day.setDate(initial_day.getDate() + 1);
            }
            weeksOfMonth.push(daysOfWeek);
        }
    }

    let isDayActive = (day) => {
        console.log(currentMonth)
        let dayMonth = day.getMonth();
        return +dayMonth === +currentMonth;
    }


    let getNextMonth = () => {
        currentYear = currentMonth === 11 ? currentYear + 1 : currentYear;
        currentMonth += 1;
        currentMonth %= 12;
        firstOfMonth = new Date(currentYear, currentMonth, 1);
        firstOfMonth = firstOfMonth.toLocaleString('default', {month: 'long'});
        buildMonth(1, currentMonth, currentYear);
    }

    let getPreviousMonth = () => {
        currentYear = currentMonth === 0 ? currentYear - 1 : currentYear;
        currentMonth -= 1;
        currentMonth %= 12;
        firstOfMonth = new Date(currentYear, currentMonth, 1);
        firstOfMonth = firstOfMonth.toLocaleString('default', {month: 'long'});
        buildMonth(1, currentMonth, currentYear);
    }
    onMount( () => {
        buildMonth(1, currentMonth, currentYear);
        firstOfMonth = new Date(currentYear, currentMonth, 1);
        firstOfMonth = firstOfMonth.toLocaleString('default', {month: 'long'});
    }
    )
</script>


<Row class="justify-content-center">
    <Col xs="3">
        <Button on:click={getPreviousMonth}>⬅</Button>
    </Col>
    <Col xs="6">
        <h2>{firstOfMonth} {currentYear}</h2>
    </Col>
    <Col xs="3">
        <Button on:click={getNextMonth}>➡</Button>
    </Col>
</Row>

<Row class="pb-3">
    <Col><h3>S</h3></Col>
    <Col><h3>M</h3></Col>
    <Col><h3>T</h3></Col>
    <Col><h3>W</h3></Col>
    <Col><h3>T</h3></Col>
    <Col><h3>F</h3></Col>
    <Col><h3>S</h3></Col>
</Row>

{#each weeksOfMonth as daysOfWeek}
	<Row class="flex">
	{#each daysOfWeek as day}
        <Day day="{day}" fade="{isDayActive(day)}" class="day"/>
    {/each}
	</Row>
{/each}

<style>
    h2 {
        color: teal;
    }
    .flex {
        display: flex;
    }
</style>