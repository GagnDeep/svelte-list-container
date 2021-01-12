<script>
  import { isHotkey } from "is-hotkey";
  import { tick, createEventDispatcher } from "svelte";

  export let autoFocus;
  export let items = [];
  export let selectedIndex = 0;
  export let stopImmediatePropagation;
  export let selectedItem;

  let listNodes = [];

  const dispatch = createEventDispatcher();

  function handleKeydown(event) {
    if (stopImmediatePropagation) {
      event.stopImmediatePropagation();
    }

    event.preventDefault();

    if (isHotkey("down", event)) {
      selectedIndex = selectedIndex + 1 < items.length ? selectedIndex + 1 : 0;
    }

    if (isHotkey("up", event)) {
      selectedIndex =
        selectedIndex - 1 > -1 ? selectedIndex - 1 : items.length - 1;
    }

    if (isHotkey("right", event)) {
      dispatch("right", { event, selectedItem, selectedIndex, items });
    }

    if (isHotkey("left", event)) {
      dispatch("left", { event, selectedItem, selectedIndex, items });
    }

    if (isHotkey("enter", event)) {
      dispatch("enter", { event, selectedItem, selectedIndex, items });
    }

    if (isHotkey("esc", event)) {
      dispatch("esc", { event, selectedItem, selectedIndex, items });
    }
  }

  function onLoad(el) {
    if (autoFocus) {
      el.focus();
    }
  }

  async function handleSelectedChange() {
    await tick();
    const target = document.querySelector(".listItemActive");

    if (target) target.scrollIntoViewIfNeeded();
  }

  $: selectedItem = items[selectedIndex];
  $: handleSelectedChange(selectedIndex);
</script>

<div use:onLoad class="outline-none overflow-auto" tabindex="0" on:keydown|capture={handleKeydown}>
    {#each items as item, i}
			<div class:listItemActive={i===selectedIndex} >
        <slot {item} {i} {selectedIndex} {selectedItem} isActive={i === selectedIndex} />
			</div>
    {/each}
</div>