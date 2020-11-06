<script>
    import { onMount } from 'svelte';
    import Day from './Day.svelte';
    import { Col, Row, Button } from 'sveltestrap';

    export let currentMonth = 9;
    export let currentYear = 2020;
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
        console.log(day)
        console.log(currentMonth)
        let dayMonth = day.getMonth();
        console.log(dayMonth)
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
    <Col xs="1">
        <Button on:click={getPreviousMonth}>Previous Month</Button>
    </Col>
    <Col xs="10">
        <h1>{firstOfMonth}</h1>
    </Col>
    <Col xs="1">
        <Button on:click={getNextMonth}>Next Month</Button>
    </Col>
</Row>
<Col>
    {#each weeksOfMonth as daysOfWeek}
        <Row>
            {#each daysOfWeek as day}
                <Col>
                    <Day day="{day.getDate()}" fade="{isDayActive(day)}"/>
                </Col>
            {/each}
        </Row>
    {/each}
</Col>

