<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
</script>

<svelte:head>
  <title>Projects</title>
</svelte:head>
<!-- <pre>{ JSON.stringify(projects, null, "\t") }</pre> -->

<h1>{ projects.length } Projects</h1>
    <!-- <nav>
        <ul>
            <li><a href="../index.html">Home</a></li>
            <li><a href="../contact/index.html">Contact</a></li>
            <li><a  href="index.html">Projects</a></li>
            <li><a href="../cv/index.html">CV</a></li>
            <li><a href="https://github.com/wenhanxue" target="_blank">GitHub</a></li>
        </ul>
    </nav> -->
    <div class="projects">
        {#each projects as p}
            <Project data={p} />

            <!-- <article>
                <h2><a href="assignments/a2/A2_report.html">{p.title}</a></h2>
                <img src={p.image} alt="">
                <p>{p.description}</p>
            </article> -->
        {/each}
        <!-- <article>
            <h2><a href="assignments/a3/A3_report.html">A3</a></h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Consequuntur impedit dolore optio voluptatum, quia culpa reiciendis. Dicta, rerum quisquam itaque assumenda optio in. Illum explicabo mollitia dolorem exercitationem, provident ab sit expedita, itaque dolore beatae quasi quae. Repellendus?</p>
        </article>
        <article>
            <h2>Minima commodi aperiam obcaecati.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Eligendi doloremque facere harum ad aspernatur! Eveniet quod omnis quisquam reiciendis hic eius necessitatibus accusantium nostrum sequi delectus expedita velit assumenda dolor eum quaerat tempora, voluptatem alias corrupti culpa totam!</p>
        </article>
        <article>
            <h2>Error nemo cupiditate expedita.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Eum quae debitis rerum! Perspiciatis voluptatum neque quasi ex, dignissimos facere repellat inventore. Fuga harum quisquam modi libero reprehenderit officia tempora, sed amet sit voluptatem quis facilis, earum placeat officiis?</p>
        </article>
        <article>
            <h2>Dolore nobis accusantium optio.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Ab dolorum at illo tenetur rem totam laboriosam! Omnis sapiente accusamus et? Sapiente voluptatibus eum possimus beatae. Voluptates corrupti pariatur illum exercitationem sit est, earum vitae officia dolor ipsa tempore!</p>
        </article>
        <article>
            <h2>Ipsa quae vero necessitatibus.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Expedita, doloribus ipsa quae est ipsam, velit maxime et quidem repudiandae corrupti esse, illum quod unde tenetur error! Deserunt vitae soluta, ipsa animi ex reprehenderit a ducimus molestiae aut ipsum?</p>
        </article>
        <article>
            <h2>Nostrum soluta corporis necessitatibus.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Rerum neque ipsa totam dignissimos aliquam debitis dolorem, non aspernatur voluptates at porro, praesentium magni fugit impedit natus, quam dolore. Cumque consectetur perspiciatis odit hic, incidunt blanditiis aliquam? Enim, quasi.</p>
        </article>
        <article>
            <h2>Quos cum nulla earum!</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Impedit inventore perspiciatis ut excepturi consequatur perferendis sit qui. Eos itaque recusandae sequi earum iusto voluptates maiores atque architecto vitae. Similique in sit assumenda accusantium fugit facilis exercitationem, magni odio.</p>
        </article>
        <article>
            <h2>Soluta beatae rem corrupti?</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Vel cupiditate saepe maxime magnam autem tempore tempora, sequi ipsam nihil molestiae? Nesciunt, inventore! Asperiores nobis adipisci mollitia quisquam id, consequuntur iusto quos, eum, temporibus accusantium nemo! Debitis, quae minus?</p>
        </article>
        <article>
            <h2>Repellat alias sequi voluptates.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Aliquam iusto nihil deserunt velit fugit fuga asperiores excepturi esse similique ducimus quo, voluptates corrupti neque atque, minima magni dolorum doloremque eum commodi modi tempore officia autem. Voluptatibus, veritatis eius.</p>
        </article>
        <article>
            <h2>Fugit delectus in quibusdam?</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Esse saepe quos amet doloribus, at perferendis eum, impedit deserunt eaque unde quibusdam? Tenetur suscipit explicabo natus quos dolor quibusdam optio eveniet nihil aspernatur nesciunt, adipisci veniam dignissimos unde iste?</p>
        </article>
        <article>
            <h2>Obcaecati praesentium sed aperiam.</h2>
            <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
            <p>Officiis dolores eius magni dolor veritatis nobis corrupti, magnam voluptas dicta quibusdam delectus at impedit. Eligendi ab molestiae a, qui dolore quidem provident numquam consectetur distinctio laudantium ex libero. Fugit!</p>
        </article> -->
    </div>