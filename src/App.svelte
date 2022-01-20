<script lang="ts">
  import { onMount } from "svelte";
  import * as d3 from "d3";

  let _svg: d3.Selection<d3.BaseType, unknown, HTMLElement, any>;
  let svg = _svg as unknown | SVGElement;
  let _btn: d3.Selection<d3.BaseType, unknown, HTMLElement, any>;
  let btn = _btn as unknown | SVGElement;

  let circle: { x: number; y: number }[] = [];

  onMount(() => {
    _svg = d3.select("svg");

    _svg
      .attr("width", 400)
      .attr("height", 400)
      .attr("style", "background: #AAA")
      .on("click", function (event: PointerEvent) {
        viewclick(event);
      });

    _btn = d3.select("button");
    _btn
      .attr("id", "reset02")
      .attr("class", "btn btn-info")
      .text("Reset")
      .on("click", function () {
        btnclick();
      });

    function viewclick(event: PointerEvent) {
      circle.push({ x: event.offsetX, y: event.offsetY });

      let color = d3.rgb(
        Math.floor(Math.random() * 255) + 1,
        Math.floor(Math.random() * 255) + 1,
        Math.floor(Math.random() * 255) + 1
      );
      let color_id = color as unknown as string;

      if (circle.length === 1) {
        _svg
          .append("circle")
          .attr("cx", circle[0].x)
          .attr("cy", circle[0].y)
          .attr("r", 15)
          .attr("fill", color_id);
      } else {
        _svg
          .append("line")
          .attr("x1", circle[0].x) // start point:x
          .attr("x2", circle[1].x) // end point:x
          .attr("y1", circle[0].y) // start point:y
          .attr("y2", circle[1].y) // end point:y
          .attr("stroke-width", 5) // line thickness
          .attr("stroke", "grey") // line color
          .attr("fill", "none");
        _svg
          .append("circle")
          .attr("cx", circle[1].x)
          .attr("cy", circle[1].y)
          .attr("r", 15)
          .attr("fill", color_id);

        circle.shift();
      }
    }
  });

  function btnclick() {
    circle = [];
    _svg.selectAll("line").remove();
    _svg.selectAll("circle").remove();
  }
</script>

<svg bind:this={svg} />
<p>
  <button bind:this={btn} />
</p>
