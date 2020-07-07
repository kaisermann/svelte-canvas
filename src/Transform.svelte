<script>
  import Layer from "./Layer.svelte";

  const VALID_METHODS = ["scale", "rotate", "translate", "transform"];

  export let methods = [],
    render = () => {},
    setup = undefined;

  $: transform = ({ context, width, height }) => {
    context.save();

    for (let [method, ...values] of methods) {
      if (VALID_METHODS.includes(method)) {
        context[method](...values);
      } else {
        console.warn(`${method} is not a valid canvas method`);
      }
    }

    render({ context, width, height });

    context.restore();
  };
</script>

<Layer {setup} render={transform} />
