<script>
  import { onMount, onDestroy } from "svelte";
  import { createPopup } from "@picmo/popup-picker";

  let txt, btn, chatSmiley, picker;

  console.log("aksh");

  const clicked = () => {
    console.log(picker);
    picker.toggle();
  };

  const typeInTextarea = (newText, el = txt) => {
    const [start, end] = [el.selectionStart, el.selectionEnd];
    el.setRangeText(newText, start, end, "select");
  };

  onMount(() => {
    console.log("mouting", chatSmiley);

    picker = createPopup(
      {
        // picker options go here
        // rootElement: chatSmiley,
        categories: ["recents", "smileys-emotion", "people-body"],
        emojiSize: "1.5rem",
        showPreview: false,
        showSearch: false,
      },
      {
        referenceElement: chatSmiley,
        triggerElement: btn,
        position: "top-end",
      }
    );

    picker.addEventListener("emoji:select", (data) => {
      console.log(data);
      // txt.value += data.emoji;
      typeInTextarea(data.emoji);
      txt.focus();
    });
  });

  onDestroy(() => {
    picker.removeEventListener("emoji:select");
  });
</script>

<div contenteditable={true} bind:this={txt} />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<button bind:this={btn} on:click={clicked} id="emoji-button">Emoji</button>
<div bind:this={chatSmiley} class="pickerContainer">container</div>

<style>
  .pickerContainer {
    border: 3px dotted;
  }
</style>
