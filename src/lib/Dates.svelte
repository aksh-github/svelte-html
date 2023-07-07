<script lang="ts">
  import { onMount } from "svelte";
  import dayjs from "dayjs";

  let curDateTasks = [], curMonthTasks = []
  let duration = "default"

  let data = {
    year: {},
    month: {}
  };

  const months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];

  const processYear = (today) => {
    const yearData = data.year;

    console.log("this month");

    Object.keys(yearData).forEach((key) => {
      if (months[today.month()] == key) {
        // console.log(yearData[`${key}`]);
        const curMonthData = yearData[`${key}`];
        Object.keys(curMonthData).forEach((dt) => {
          // console.log(dt + '-' + months[today.month()], curMonthData[dt])
          curMonthData[dt].forEach((d)=> {
            // if(d.startsWith('.')) console.log('process')
            d = d.startsWith('.') ? data.$[`${d.replace('.', '')}`] : d
            curMonthTasks = [...curMonthTasks, {item: dt + '-' + months[today.month()] + ": " + d}]
          })
          
        })

      }
    });
  };

  const processMonth = (today, b4, after) => {
    const monthData = data.month;

    // console.log(b4.date() + "-" + months[b4.month()], "to", after.date() + "-" + months[after.month()]);
    duration = b4.date() + "-" + months[b4.month()] + " to " + after.date() + "-" + months[after.month()]

    Object.keys(monthData).forEach((key) => {
      let dt = dayjs();

      dt = dt.set("date", parseInt(key, 10));

      const daysPast = dt.diff(today, "day");

      if (dt.isBefore(after) && dt.isAfter(b4)) {

        const curMonthData = monthData[key];
        curMonthData.forEach((d) => {
          d = d.startsWith('.') ? data.$[`${d.replace('.', '')}`] : d
          console.log(d)
          curDateTasks = [...curDateTasks, {item: dt.date() + '-' + months[today.month()] + ": " + d}]
        })
      }
        
        
    });
  };

  onMount(async() => {
    const today = dayjs();

    // console.log(months[today.month()]);

    fetch('/data.json').then(resp=>resp.json()).then((resp)=> {
      console.log(resp)
      data = resp;
      const withinDays = 5;

    const b4 = today.subtract(withinDays, "day"),
      after = today.add(withinDays, "day");

    // for yearly once
    processYear(today);

    // for monthly once
    processMonth(today, b4, after);
  });

  const getColor = (daysPast: number) => {
    switch (true) {
      case daysPast < 0:
        return "red";
      case daysPast === 0:
        return "orange";
      default:
        return null;
    }
  };
    })

</script>

<div>
<h3>Hello dates</h3>

<h3>{duration}</h3>
<ul>
{#each curDateTasks as task}
  <li style={`color: ${task.bgcolor};`}>{task.item}</li>
{/each}
</ul>

<hr />
<h3>Current Month</h3>
<ul>
  {#each curMonthTasks as task}
    <li style={`color: ${task.bgcolor};`}>{task.item}</li>
  {/each}
  </ul>

</div>


<style>
  
li {
    text-align: initial;
    padding: .5em;
    border: 2px dotted;
    list-style: none;
}
</style>