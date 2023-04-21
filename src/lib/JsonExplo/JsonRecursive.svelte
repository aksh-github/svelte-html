<script>
  import Folder from "./Folder.svelte";

  let root2 = {
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

  let root = {
    $: "template",
    tp: false,
    props: { version: "1.0" },
    children: [
      {
        $: "div",
        props: { className: "chat-container" },
        children: [
          {
            $: "header",
            props: {},
            children: [{ $: "div", props: {}, children: ["Room: {props.room}"] }],
          },
          {
            $: "div",
            props: { className: "chat-row" },
            children: [
              {
                $: "div",
                props: { className: "messages" },
                children: [
                  {
                    $: "For",
                    props: {
                      custom: '{"each":"${@g.messages}","as":"div"}',
                    },
                    children: [
                      {
                        $: "div",
                        props: {},
                        children: [
                          {
                            $: "div",
                            props: {
                              className: "me  w3-animate-bottom",
                              custom: '{"hide": {"!=": ["me", "${@l.from}"]}}',
                            },
                            children: [
                              {
                                $: "span",
                                props: {},
                                children: ["${@l.message}"],
                              },
                            ],
                          },
                          {
                            $: "div",
                            props: {
                              className: "other  w3-animate-top",
                              custom: '{"hide": {"==": ["me", "${@l.from}"]}}',
                            },
                            children: [
                              {
                                $: "p",
                                props: {},
                                children: ["${@l.from}"],
                              },
                              {
                                $: "span",
                                props: {},
                                children: ["${@l.message}"],
                              },
                            ],
                          },
                        ],
                      },
                    ],
                  },
                ],
              },
            ],
          },
          {
            $: "footer",
            props: {},
            children: [
              {
                $: "textarea",
                props: {
                  name: "txtmsg",
                  rows: "8",
                  placeholder: "Type your message...",
                },
                children: [],
              },
              {
                $: "button",
                props: { disabled: "${@g.sendBtnFlag}" },
                children: [
                  {
                    $: "div",
                    props: { className: "wrapper" },
                    children: ["✈️"],
                  },
                ],
              },
            ],
          },
        ],
      },
    ],
  };

  let path = "";

  const eleClicked = (e) => {
    // console.log(e, e.target);
    path = "root" + e.target?.getAttribute("name");
    e.stopPropagation();
  };
</script>

<input bind:value={path} />
<div class="explorer" on:click={eleClicked}>
  <Folder displayName="$" files={root2} parentName="" expanded />
</div>

<style>
  .explorer {
    text-align: initial;
  }
</style>
