<script context="module">
  export async function preload({ params }) {
    const res = await this.fetch(`/projects/${params.slug}.json`);
    const data = await res.json();
    if (res.status === 200) {
      return { post: data };
    } else {
      this.error(res.status, data.message);
    }
  }
</script>

<!-- Layout for all the projects -->
<script lang="ts">
  import Header from "../../components/Header.svelte";
  import type { ProjectPage } from "../../_types";

  export let post: ProjectPage;
  post.html = post.html.replace(/<a/g, '<a target="_blank"');
</script>

<svelte:head>
  <meta
    name="title"
    content="{post.metadata.title} | LaSpruca Semi-Professional Googler"
  />
  <meta name="description" content={post.metadata.description} />
  <title>{post.metadata.title} | LaSpruca Semi-Professional Googler</title>
</svelte:head>

<Header />

<section class="content">
  <div class="title-block">
    <h1>{post.metadata.title}</h1>
    <p><small>{post.metadata.dataString}</small></p>
    <div class="links">
      {#each post.metadata.subLinks as link}
        <a href={link.href}>{link.text}</a>
      {/each}
    </div>
  </div>
  <div class="body">
    {@html post.html}
  </div>
</section>

<style lang="scss">
  @import "../../assets/style/routes/projects/[slug].scss";
</style>
