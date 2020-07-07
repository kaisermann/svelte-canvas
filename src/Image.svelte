<script>
  import Transform from "./Transform.svelte";

  export let src,
    x = 0,
    y = 0,
    scale = 1,
    position = "topleft";

  let width, height, methods;
  let canvasWidth, canvasHeight;

  const image = new Image();
  image.onload = () => {
    console.log(canvasWidth, canvasHeight);
    if (image.height < canvasHeight) {
      height = canvasHeight;
      width = image.width * (canvasHeight / image.height);
    } else {
      width = canvasWidth;
      height = image.height * (canvasWidth / image.width);
    }
  };
  image.src = src;

  $: {
    if (position === "center")
      methods = [
        ["translate", x - width / 2, y - height / 2],
        ["scale", scale, scale]
      ];
    else if (position === "topleft") {
      methods = [["translate", x, y], ["scale", scale, scale]];
    }
  }

  $: setup = ({ width, height }) => {
    canvasWidth = width;
    canvasHeight = height;
  };

  $: render = ({ context }) => {
    context.drawImage(image, 0, 0, width, height);
  };
</script>

<Transform {setup} {render} {methods} />
