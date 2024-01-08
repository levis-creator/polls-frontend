<script>
  export let type = "primary";
  export let flat = false;
  export let inverse = false;
</script>

<button
  class={`${
    type === "primary"
      ? "bg-red-600 text-white"
      : type === "secondary"
        ? "bg-green-500 text-white"
        : ""
  }
  ${flat && "shadow-none"}
  ${
    type === "primary" &&
    inverse &&
    "text-red-600 bg-white border-2 border-solid border-red-600"
  }
  ${
    type === "secondary" &&
    inverse &&
    "text-green-600 bg-white border-2 border-solid border-green-600"
  }
  } cursor-pointer rounded-md py-2 px-3 font-bold shadow-md `}
  on:click
>
  <slot />
</button>
