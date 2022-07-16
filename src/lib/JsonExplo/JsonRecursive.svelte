<script>
  import Folder from "./Folder.svelte";

  let root2 = {
    a: "this is for a",
    b: "this is for b",
    d: {
      d1: 10,
      d2: "this is for d2",
      d31: "this is for d1",
      d42: "this is for d2",
      d51: "this is for d1",
      d52: "this is for d2",
      d61: "this is for d1",
      d62: "this is for d2",
    },
    fdd: [10, 20],
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
  <Folder displayName="$" files={root} parentName="" expanded />
</div>

<style>
  .explorer {
    text-align: initial;
  }
</style>
