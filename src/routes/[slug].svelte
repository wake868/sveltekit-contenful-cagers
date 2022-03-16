<script context="module">
  import { documentToHtmlString } from "@contentful/rich-text-html-renderer";
  import { gql, GraphQLClient } from "graphql-request";

  export async function load({ fetch, params }) {
    const env = import.meta.env;

    const graphcms = new GraphQLClient(`${env.VITE_GRAPHCMS_URL}`, {
      headers: {},
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
          }
        }
      }
    `;

    const { article } = await graphcms.request(query);
    return {
      props: {
        article,
      },
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
        <img src={photo.url} alt="basketball" />
      </a>
    {/each}
  </div>
</div>

<style>
  .back {
    margin: 20px auto;
    max-width: 1057px;
  }
  .article {
    margin-bottom: 15px;
    background-color: white;
    border-radius: 5px;
    padding: 15px;
    margin: 5px auto;
    max-width: 1024px;
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
    max-width: 225px;
  }
  /* media queries for xl screens */
  @media (min-width: 1450px) {
    .photos img {
      max-width: 275px;
    }
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
    .photos img {
      max-width: 275px;
    }
  }
</style>
