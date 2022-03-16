<script context="module">
  import { gql, GraphQLClient } from "graphql-request";

  export async function load({ fetch, process }) {
    const env = import.meta.env;
    // const res = await fetch(
    //   `https://cdn.contentful.com/spaces/pukk0vn61rq1/environments/master/entries?content_type=article&order=fields.rank&access_token=07fdb4b77671187eeb5b4551e6b601e8390575d5433244d1bc8334e826cb3c1b`
    // );
    // const { items } = await res.json();

    // return {
    //   props: {
    //     articles: items,
    //   },
    // };
    const graphcms = new GraphQLClient(`${env.VITE_GRAPHCMS_URL}`, {
      headers: {},
    });

    const query = gql`
      query AllArticles {
        articles(orderBy: rank_ASC) {
          id
          slug
          title
          blurb
        }
      }
    `;

    const { articles } = await graphcms.request(query);
    return {
      props: {
        articles,
      },
    };
  }
</script>

<script>
  export let articles;
  // console.log(articles);
</script>

<div class="articles">
  {#each articles as article}
    <div class="article">
      <h3>{article.title}</h3>
      <hr />
      <p>{article.blurb}</p>
      <a class="button" href="/{article.slug}">VIEW</a>
    </div>
  {/each}
</div>

<style>
  .articles {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    margin: 5px auto;
    padding: 0 5px;
  }
  .article {
    position: relative;
    margin-bottom: 15px;
    background-color: white;
    border-radius: 5px;
    padding: 5px;
    min-height: 175px;
  }
  .article h3 {
    color: var(--dark);
  }
  .article hr {
    height: 1px;
    border: none;
    background: gray;
  }
  .article p {
    font-size: 0.9em;
  }
  .button {
    position: absolute;
    bottom: 0;
    right: 0;
  }

  /* media queries for smaller screens */
  @media (max-width: 1024px) {
    .articles {
      grid-template-columns: repeat(3, 1fr);
    }
  }
  @media (max-width: 860px) {
    .articles {
      grid-template-columns: repeat(2, 1fr);
    }
  }
  @media (max-width: 575px) {
    .articles {
      grid-template-columns: repeat(1, 1fr);
    }
    .article {
      min-height: 150px;
    }
  }
</style>
