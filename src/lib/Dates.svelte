<script lang="ts">
  import { onMount } from "svelte";
  import dayjs from "dayjs";

  let curDateTasks = [],
    curMonthTasks = [];
  let duration = "default";
  let todayDate = dayjs();

  let data = {
    $: {},
    year: {},
    month: {},
    custom: {},
  };

  const days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
  const months = [
    "Jan",
    "Feb",
    "Mar",
    "Apr",
    "May",
    "Jun",
    "Jul",
    "Aug",
    "Sep",
    "Oct",
    "Nov",
    "Dec",
  ];

  const processYear = (today) => {
    const yearData = data.year;

    console.log("this month");

    Object.keys(yearData).forEach((key) => {
      if (months[today.month()] == key) {
        // console.log(yearData[`${key}`]);
        const curMonthData = yearData[`${key}`];
        Object.keys(curMonthData).forEach((dt) => {
          // console.log(dt + '-' + months[today.month()], curMonthData[dt])
          curMonthData[dt].forEach((d) => {
            // if(d.startsWith('.')) console.log('process')
            d = d.startsWith(".") ? data.$[`${d.replace(".", "")}`] : d;
            curMonthTasks = [
              ...curMonthTasks,
              { item: dt + "-" + months[today.month()] + ": " + d },
            ];
          });
        });
      }
    });
  };

  const processMonth = (b4, after) => {
    const monthData = data.month;

    // console.log(b4.date() + "-" + months[b4.month()], "to", after.date() + "-" + months[after.month()]);
    duration =
      b4.date() +
      "-" +
      months[b4.month()] +
      " to " +
      after.date() +
      "-" +
      months[after.month()];

    Object.keys(monthData).forEach((key) => {
      let dt = dayjs();

      dt = dt.set("date", parseInt(key, 10));

      const daysPast = dt.diff(todayDate, "day");

      if (dt.isBefore(after) && dt.isAfter(b4)) {
        const curMonthData = monthData[key];
        curMonthData.forEach((d) => {
          d = d.startsWith(".") ? data.$[`${d.replace(".", "")}`] : d;
          // console.log(d)
          curDateTasks = [
            ...curDateTasks,
            { item: dt.date() + "-" + months[todayDate.month()] + ": " + d },
          ];
        });
      }
    });
  };

  const processMonthCustom = (b4, after) => {
    const dateProcess = (_dt) => {
      let flag = false;
      _dt = _dt.replace("/", "");
      _dt = parseInt(_dt, 10);
      // console.log(todayDate.date() % _dt)
      if (todayDate.date() % _dt == 0) {
        return true;
      }

      return flag;
    };

    const dayProcess = (_dt) => {
      let flag = false;
      const arr = _dt.split(",");
      arr.forEach((datum) => {
        if (days[todayDate.day()] === datum) {
          // console.log(datum)
          flag = true;
          return;
        }
      });

      return flag;
    };

    const { custom } = data;

    // console.log(custom)

    Object.keys(custom).forEach((k) => {
      const freq = k.split(":")[0];
      const dta = k.split(":")[1];

      let flag = false;

      let arr = custom[k];
      console.log(arr);

      arr.forEach((d) => {
        switch (true) {
          case freq.startsWith("w"):
            // console.log('week process', dta)
            flag = dayProcess(dta);
            if (flag) {
              d = d.startsWith(".") ? data.$[`${d.replace(".", "")}`] : d;
              // console.log(d)
              curDateTasks = [
                ...curDateTasks,
                {
                  item:
                    todayDate.date() +
                    "-" +
                    months[todayDate.month()] +
                    ": " +
                    d,
                },
              ];
            }
            break;
          case freq.startsWith("m"):
            // console.log('date process', dta)
            flag = dateProcess(dta);
            if (flag) {
              d = d.startsWith(".") ? data.$[`${d.replace(".", "")}`] : d;
              // console.log(d)
              curDateTasks = [
                ...curDateTasks,
                {
                  item:
                    todayDate.date() +
                    "-" +
                    months[todayDate.month()] +
                    ": " +
                    d,
                },
              ];
            }

            break;
          default:
            // console.log(data.custom[k])
            console.log("not sure");
        }
      });
    });
  };

  onMount(async () => {
    // const today = dayjs();

    // console.log(months[today.month()]);

    fetch("/data.json")
      .then((resp) => resp.json())
      .then((resp) => {
        console.log(resp);
        data = resp;
        const withinDays = 3;

        const b4 = todayDate.subtract(withinDays, "day"),
          after = todayDate.add(withinDays, "day");

        // for yearly once
        processYear(todayDate);

        // for monthly once
        processMonth(b4, after);

        // for monthly custom
        processMonthCustom(b4, after);
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
  });
</script>

<div>
  <h3>{todayDate?.date() + "-" + months[todayDate?.month()]}</h3>

  <h3>{duration}</h3>
  <ul>
    {#each curDateTasks as task}
      <li style={`color: ${task.bgcolor};`}>{task.item}</li>
    {/each}
  </ul>

  <hr />
  <h3>Current Month: {months[todayDate.month()]}</h3>
  <ul>
    {#each curMonthTasks as task}
      <li style={`color: ${task.bgcolor};`}>{task.item}</li>
    {/each}
  </ul>
</div>

<style>
  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    text-align: initial;
    padding: 0.5em;
    border: 2px dotted;
    list-style: none;
  }
</style>
