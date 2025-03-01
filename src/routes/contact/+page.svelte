<svelte:head>
  <title>Contact me</title>
</svelte:head>

<h1>Contact</h1>
<!-- <nav>
    <ul>
        <li><a href="../index.html">Home</a></li>
        <li><a href="index.html">Contact</a></li>
        <li><a href="../projects/index.html">Projects</a></li>
        <li><a href="../cv/index.html">CV</a></li>
        <li><a href="https://github.com/wenhanxue" target="_blank">GitHub</a></li>
    </ul>
</nav> -->
<form action="mailto:wx100@wellesley.edu" method="post" enctype="text/plain">
    <label>Email: <input name="email" type="email"></label>
    <label>Subject: <input name="subject"></label>
    <label>Message: <textarea name="body"></textarea></label>
    <button>Send</button>
</form>