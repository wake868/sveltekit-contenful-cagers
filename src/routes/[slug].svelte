<script context="module">
  import { documentToHtmlString } from "@contentful/rich-text-html-renderer";
  import { gql, GraphQLClient } from "graphql-request";

  export async function load({ fetch, params }) {
    const env = import.meta.env;

    const graphcms = new GraphQLClient(`${env.VITE_GRAPHCMS_URL}`, {
      headers: {}
    });

    const query = gql`
      query MyQuery {
        article(where: { slug: "${params.slug}" }) {
          id
          title
          slug
          body {
            html
          }
          photos {
            url
            handle
          }
        }
      }
    `;

    const { article } = await graphcms.request(query);
    return {
      props: {
        article
      }
    };
  }
</script>

<script>
  export let article;
</script>

<div class="back">
  <a class="button" href="/">BACK</a>
</div>
<div class="article">
  <h2>{article.title}</h2>
  <hr />
  <div id="rich-text-body">
    {@html article.body.html}
  </div>
  <div class="photos">
    {#each article.photos as photo}
      <a href={photo.url} target="_blank">
        <img
          src="https://media.graphcms.com/resize=fit:clip,width:250/output=format:webp/{photo.handle}"
          alt="basketball"
        />
      </a>
    {/each}
  </div>
</div>

<style>
  .back {
    margin: 20px auto;
  }
  .article {
    margin-bottom: 15px;
    background-color: white;
    border-radius: 5px;
    padding: 15px;
    margin: 5px auto;
  }
  .article hr {
    height: 1px;
    border: none;
    background: gray;
    margin: 0;
  }
  #rich-text-body {
    margin-top: 25px;
  }
  .photos {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    justify-items: center;
    margin-top: 25px;
  }
  .photos img {
    /* max-width: 225px; */
    max-width: 98%;
  }
  /* media queries for smaller screens */
  @media (max-width: 1190px) {
    .photos {
      grid-template-columns: repeat(4, 1fr);
    }
  }
  @media (max-width: 943px) {
    .photos {
      grid-template-columns: repeat(3, 1fr);
    }
  }
  @media (max-width: 720px) {
    .photos {
      grid-template-columns: repeat(2, 1fr);
    }
  }
  @media (max-width: 495px) {
    .photos {
      grid-template-columns: repeat(1, 1fr);
    }
  }
</style>
