<script>
import * as d3 from 'd3';

export let lines = [];
export let width = 400;
export let colorScale = d3.scaleOrdinal(d3.schemeTableau10);
export let svgWidth = 1200;

let svg;
let files = [];
let previousDotCounts = new Map();

const firstColumnWidth = 150;
const fileInfoMargin = 100;
const dotsColumnX = firstColumnWidth + fileInfoMargin;
const approxDotWidth = 20;
const linesPerDot = 1;
const baseY = 10;
const totalLinesOffset = 20;
const fileInfoHeight = baseY + totalLinesOffset;
const dotRowHeight = 20;


$: files = d3.groups(lines, d => d.file)
            .map(([name, lines]) => ({ name, lines }))
            .sort((a, b) => b.lines.length - a.lines.length);

function generateDots(file, svgWidth) {
    const totalDots = Math.ceil(file.lines.length / linesPerDot);
    const availableWidth = svgWidth - dotsColumnX;
    const maxDotsPerRow = Math.floor(availableWidth / approxDotWidth) || totalDots;
    let tspans = "";
    const dotRows = Math.ceil(totalDots / maxDotsPerRow);
    for (let r = 0; r < dotRows; r++) {
        const rowLines = file.lines.slice(r * maxDotsPerRow, (r + 1) * maxDotsPerRow);
        const rowDots = rowLines
            .map(line => `<tspan style="fill:${colorScale(line.type)}">•</tspan>`)
            .join('');
        tspans += `<tspan x="${dotsColumnX}" dy="${r === 0 ? 0 : dotRowHeight + 'px'}">${rowDots}</tspan>`;
    }
    return tspans;
}

$: filesWithHeights = files.map(file => {
  const totalDots = Math.ceil(file.lines.length / linesPerDot);
  const availableWidth = width - dotsColumnX;
  const maxDotsPerRow = Math.floor(availableWidth / approxDotWidth) || totalDots;
  const dotRows = Math.ceil(totalDots / maxDotsPerRow);
  return { ...file, groupHeight: fileInfoHeight + dotRows * dotRowHeight };
});

$: positions = (() => {
  let pos = [], y = 0;
  for (const f of filesWithHeights) {
    pos.push(y);
    y += f.groupHeight;
  }
  return pos;
})();


$: if (svg) {
    // const svgWidth = 1200;
    const totalHeight = positions.length
        ? positions[positions.length - 1] + filesWithHeights[filesWithHeights.length - 1].groupHeight
        : 0;
    d3.select(svg)
        .attr('width', svgWidth)
        .attr('height', totalHeight)
        .style('overflow', 'hidden');

    const groups = d3.select(svg)
    .selectAll('g.file')
    .data(filesWithHeights, d => d.name);

    groups.exit().remove();

    const enterGroups = groups.enter()
    .append('g')
    .attr('class', 'file')
    .attr('transform', (d, i) => `translate(0, ${positions[i]})`);

    enterGroups.append('text')
    .attr('class', 'filename')
    .attr('x', 10)
    .attr('y', baseY)
    .attr('dominant-baseline', 'hanging')
    .text(d => d.name);

    enterGroups.append('text')
        .attr('class', 'linecount')
        .attr('x', 10) 
        .attr('y', baseY + totalLinesOffset)
        .attr('dominant-baseline', 'hanging')
        .text(d => `${d.lines.length} lines`);

    groups.select('text.linecount')
        .text(d => `${d.lines.length} lines`)
        .attr('y', baseY + totalLinesOffset)
        // .attr('x', 250);

    enterGroups.append('text')
        .attr('class', 'unit-dots')
        .attr('x', dotsColumnX)
        .attr('y', baseY + totalLinesOffset)
        .attr('dominant-baseline', 'mathematical')
        .attr('fill', "#1f77b4")
        .html(d => generateDots(d, svgWidth));

    groups.attr('transform', (d, i) => `translate(0, ${positions[i]})`)
        .select('text.filename')
        .attr('y', baseY)
        .text(d => d.name);

    groups.select('text.unit-dots')
        .html(d => generateDots(d, svgWidth))
        .attr('x', dotsColumnX)
        .attr('y', baseY + totalLinesOffset)
        .attr('fill', "#1f77b4");

    groups.each(function(d) {
        const groupSel = d3.select(this);
        const unitDotsSel = groupSel.select('text.unit-dots');
        const newCount = d.lines.length;
        const oldCount = previousDotCounts.get(d.name) || 0;
        
        unitDotsSel.html(generateDots(d, svgWidth));
        
        if(newCount > oldCount) {
            unitDotsSel.selectAll('tspan.dot')
                .filter(function() {
                    return +this.getAttribute('data-index') >= oldCount;
                })
                .attr('opacity', 0)
                .transition()
                .duration(1000)
                .ease(d3.easeCubicOut)
                .attr('opacity', 1)
        }
        
        previousDotCounts.set(d.name, newCount);
    });

    groups
        .transition()
        .duration(function(d, i) {
            const currentTransform = this.getAttribute("transform") || "translate(0,0)";
            const match = currentTransform.match(/translate\(\s*0\s*,\s*([0-9.]+)\s*\)/);
            const oldY = match ? +match[1] : 0;
            const newY = positions[i];
            const distance = Math.abs(newY - oldY);
            return distance * 2;
        })
        .attr('transform', (d, i) => `translate(0, ${positions[i]})`);

}   
</script>

<svg bind:this={svg}/>

<style>
    :global(text.unit-dots) {
        font-size: 2.2rem;
    }

    :global(text.linecount) {
        font-size: 0.85rem;
        fill: #666;
        font-style: italic;
    }

    :global(text.filename) {
        font-size: 1rem;
        font-weight: 500;
        fill: #333; /* Dark text for filenames */
    }
</style>