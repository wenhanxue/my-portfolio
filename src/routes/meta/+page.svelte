<script>
    import * as d3 from "d3";
    import { onMount } from "svelte";
    import {
        computePosition,
        autoPlacement,
        offset,
    } from '@floating-ui/dom';
    import Bar from '$lib/Bar.svelte';



    //Importing data
    let data = [];
    let commits = [];
    let files = [];

    onMount(async () => {
        data = await d3.csv("/loc.csv", row => ({
            ...row,
            line: Number(row.line), // or just +row.line
            depth: Number(row.depth),
            length: Number(row.length),
            date: new Date(row.date + "T00:00" + row.timezone),
            datetime: new Date(row.datetime)
        }));

        commits = d3.groups(data, d => d.commit).map(([commit, lines]) => {
            let first = lines[0];
            let {author, date, time, timezone, datetime} = first;
            let ret = {
                id: commit,
                url: "https://github.com/vis-society/lab-7/commit/" + commit,
                author, date, time, timezone, datetime,
                hourFrac: datetime.getHours() + datetime.getMinutes() / 60,
                totalLines: lines.length
            };

            // Like ret.lines = lines
            // but non-enumerable so it doesn’t show up in JSON.stringify
            Object.defineProperty(ret, "lines", {
                value: lines,
                configurable: true,
                writable: true,
                enumerable: false,
            });
            return ret;
        });

        commits = d3.sort(commits, d => -d.totalLines); // Sort by size (descending)
        
        files = Array.from(d3.group(data, d => d.file));
    });

    //Scatterplot
    let width = 1000, height = 600;
    let margin = {top: 10, right: 10, bottom: 30, left: 20};
    let usableArea = {
        top: margin.top,
        right: width - margin.right,
        bottom: height - margin.bottom,
        left: margin.left
    };
    let xAxis, yAxis, yAxisGridlines;

    $: minDate = d3.min(commits.map(d => d.date));
    $: maxDate = d3.max(commits.map(d => d.date));
    $: maxDatePlusOne = new Date(maxDate);
    $: maxDatePlusOne.setDate(maxDatePlusOne.getDate() + 1);

    $: xScale = d3.scaleTime()
                .domain([minDate, maxDatePlusOne])
                .range([usableArea.left, usableArea.right])
                .nice();

    $: yScale = d3.scaleLinear()
                .domain([24, 0])
                .range([usableArea.bottom, usableArea.top]);

    usableArea.width = usableArea.right - usableArea.left;
    usableArea.height = usableArea.bottom - usableArea.top;

    $: {
        d3.select(xAxis).call(d3.axisBottom(xScale));
        d3.select(yAxis).call(d3.axisLeft(yScale).tickFormat(d => String(d % 24).padStart(2, "0") + ":00"));
    };

    $: {
        d3.select(yAxisGridlines).call(
            d3.axisLeft(yScale)
            .tickFormat("")
            .tickSize(-usableArea.width)
        );
    };

    //adding tooltip for the scatterplot
    let hoveredIndex = -1;

    $: hoveredCommit = commits[hoveredIndex] ?? hoveredCommit ?? {};

    //bulletproof positioning of tooltip
    let commitTooltip;
    let tooltipPosition = {x: 0, y: 0};

    let clickedCommits = []; //clicakble commits

    async function dotInteraction (index, evt) {
        let hoveredDot = evt.target;
        if (evt.type === "mouseenter") {
            hoveredIndex = index;
            tooltipPosition = await computePosition(hoveredDot, commitTooltip, {
                strategy: "fixed", // because we use position: fixed
                middleware: [
                    offset(5), // spacing from tooltip to dot
                    autoPlacement() // see https://floating-ui.com/docs/autoplacement
                ],
            });        }
        else if (evt.type === "mouseleave") {
            hoveredIndex = -1
        }

        //clickable commits
        else if (evt.type === "click") {
            let commit = commits[index]
            if (!clickedCommits.includes(commit)) {
                // Add the commit to the clickedCommits array
                clickedCommits = [...clickedCommits, commit];
            }
            else {
                    // Remove the commit from the array
                    clickedCommits = clickedCommits.filter(c => c !== commit);
            }
        }
    };

    //Circles to scale
    $: rExtent = d3.extent(commits, d => d.totalLines);
    $: rScale = d3.scaleSqrt()
        .domain(rExtent)
        .range([2, 30]);

    //Bar chart
    $: allTypes = Array.from(new Set(data.map(d => d.type)));
    $: selectedLines = (clickedCommits.length > 0 ? clickedCommits : commits).flatMap(d => d.lines);
    $: selectedCounts = d3.rollup(
        selectedLines,
        v => v.length,
        d => d.type
    );
    $: languageBreakdown = allTypes.map(type => [type, selectedCounts.get(type) || 0]);
</script>

<h1>Meta</h1>
<p>This page includes stats about the code of this website</p>

<h2>Repo Summary</h2>
<section>
    <dl class="stats">
        <dt>Total <abbr title="Lines of code">LOC</abbr></dt>
        <dd>{data.length}</dd>
        <dt>Commits</dt>
        <dd>{commits.length}</dd>
        <dt>Files</dt>
        <dd>{files.length}</dd>
    </dl>
</section>

<h3>Commits by time of day</h3>
<svg viewBox="0 0 {width} {height}">
	<g class="dots">
        {#each commits as commit, index }
            <circle class:selected={ clickedCommits.includes(commit) }
                on:click={ evt => dotInteraction(index, evt) }
                on:mouseenter={evt => dotInteraction (index, evt)}
                on:mouseleave={evt => dotInteraction (index, evt)}
                cx={ xScale(commit.datetime) }
                cy={ yScale(commit.hourFrac) }
                r={rScale(commit.totalLines)}
                fill="steelblue"
            />
        {/each}
    </g>

    <g transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />
    <g class="gridlines" transform="translate({usableArea.left}, 0)" bind:this={yAxisGridlines} />
    <g transform="translate({usableArea.left}, 0)" bind:this={yAxis} />
</svg>

<Bar data={languageBreakdown} width={width} />

<dl class="info tooltip" bind:this={commitTooltip} hidden={hoveredIndex === -1} style="top: {tooltipPosition.y}px; left: {tooltipPosition.x}px">
    <dt>Commit</dt>
    <dd><a href="{ hoveredCommit.url }" target="_blank">{ hoveredCommit.id }</a></dd>

    <dt>Date</dt>
    <dd>{ hoveredCommit.datetime?.toLocaleString("en", {dateStyle: "full"}) }</dd>

    <dt>Time</dt>
    <dd>{ hoveredCommit.datetime?.toLocaleString("en", {
        hour: '2-digit',
        minute: '2-digit',
        timeZoneName: 'short'
    }) }</dd>

    <dt>Author</dt>
    <dd>{ hoveredCommit.author }</dd>

    <dt>Lines</dt>
    <dd>{ hoveredCommit.totalLines?.toLocaleString()}</dd>
</dl>    


<style>
    svg {
		overflow: visible;
	}

    .gridlines {
	    stroke-opacity: .2;
    }

    dl.info {
        display: grid;
        grid-template-columns: auto 1fr; /* 1st col for dt, 2nd for dd */
        gap: 0.5rem 0.8rem; /* row-gap column-gap */
        margin: 0; /* Remove default margins */
        padding: 1rem;
        align-items: baseline;
        border-radius: 8px; /* Subtle rounded corners */
        box-shadow: 0 2px 8px rgba(0,0,0,0.1); /* Minimal shadow */
        border: 1px solid #eee; /* Light border */
        background: rgba(255, 255, 255, 0.3); /* Light theme base */
        backdrop-filter: blur(5px);

        /* for making tooltip only appear when hovered over a dot */
        transition-duration: 400ms;
        transition-property: opacity, visibility;

        &[hidden]:not(:hover, :focus-within) {
            opacity: 0;
            visibility: hidden;
        }
    }

    dl.info dt {
        font-weight: 600;
        font-size: 0.9em;
        text-align: right;
    }

    dl.info dd {
        margin: 0;
        font-size: 0.85em;
    }

    .tooltip{
        position: fixed;
        top: 1em;
        left: 1em;
    }

    circle {
        transition: 200ms;
        transform-origin: center;
        transform-box: fill-box;
        fill-opacity: 0.6;


        &:hover {
            transform: scale(1.5);
        }
    }

    .selected {
        fill: var(--color-accent);
    }


    section {
        margin-bottom: 3rem;
    }

    dl.stats {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-template-rows: auto auto;
        gap: 10px;
        /* background-color: var(--background-color);
        color: var(--text-color); */
        padding: 1em;
        border-radius: 8px;
        border: 1px solid;
    }

    dl.stats dt {
        grid-row: 1;
        font-weight: bold;
        /* color: var(--dt-color); */
    }

    dl.stats dd {
        grid-row: 2;
        margin: 0;
        /* color: var(--dd-color); */
    }
</style>