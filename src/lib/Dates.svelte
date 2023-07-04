<script lang="ts">
  import { onMount } from "svelte";
  import dayjs from "dayjs";

  let curDateTasks = [], curMonthTasks = []

  const data = {
    "0": {
      "5": [],
    },
    year: {
      Feb: {
        "8": ["Baba BDay"],
      },
      Mar: {
        "3": ["Investment to Gdrive"],
        "8": ["Investment to Gdrive"],
        "15": ["Investment to Gdrive"],
        "22": ["Investment to Gdrive"],
      },
      Apr: {
        "5": ["Upload half yearly HDFC bank statement, PPF statement"],
        "15": ["Upload half yearly HDFC bank statement, PPF statement"],
        "25": ["Upload half yearly HDFC bank statement, PPF statement", "Get 26AS, AIS, TIS"],
      },
      May: {
        "25": ["Check and upload Form16", "Get 26AS, AIS, TIS"],
      },
      Jun: {
        "15": ["Check and upload Form16", "Get 26AS, AIS, TIS"],
        "25": ["Check and upload Form16", "Get 26AS, AIS, TIS"],
      },
      Jul: {
        "15": ["Check and upload Form16", "Get 26AS, AIS, TIS"],
        "25": ["Check and upload Form16", "Get 26AS, AIS, TIS"],
      },
      Aug: {
        "6": ["Ak BDay", "Renew PUC"],
        "15": ["Ruta Bday", "Renew PUC"],
      },
      Dec: {
        "24": ["Pay SBI Insurance"],
      },
      Oct: {
        "5": ["Upload half yearly HDFC bank statement"],
        "15": ["Upload half yearly HDFC bank statement"],
        "25": ["Upload half yearly HDFC bank statement"],
      },
    },
    month: {
      "1": ["Vodafone Bill"],
      "30": ["Electricity Bill", "M Gas Bill", "Declare investments to TCS"],
      "5": ["Upload Salary slip, Investment to Gdrive"],
    },
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
            curMonthTasks = [...curMonthTasks, {item: dt + '-' + months[today.month()] + ": " + d}]
          })
          
        })

      }
    });
  };

  const processMonth = (today, b4, after) => {
    const monthData = data.month;

    console.log(b4.date() + "-" + months[b4.month()], "to", after.date() + "-" + months[after.month()]);

    Object.keys(monthData).forEach((key) => {
      let dt = dayjs();

      dt = dt.set("date", parseInt(key, 10));

      const daysPast = dt.diff(today, "day");

      if (dt.isBefore(after) || dt.isAfter(b4)) {
        let item = dt.date() + ": " + monthData[key]
        console.log(item);
        curDateTasks = [...curDateTasks, {item: item, bgcolor: getColor(daysPast)}]
      }
        
        
    });
  };

  onMount(() => {
    const today = dayjs();

    // console.log(months[today.month()]);

    const withinDays = 3;

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
</script>

<div>
<h3>Hello dates</h3>

<h3>+/- 3 days</h3>
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
}
</style>