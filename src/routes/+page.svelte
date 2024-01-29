<script type="module">
    import { Octokit } from "octokit";

    const octo = new Octokit({});

    const repos_req = octo.request("GET /users/{username}/repos", {
        username: "spflaumer",
    });
</script>

<main>
    <h2>My Repositories</h2>
    <div class="repos">
        {#await repos_req}
            <p>Loading repos</p>
        {:then repos}
            {#each repos.data as repo}
                {#if !repo.fork}
                    <div class="repo">
                        <p>{repo.name}</p>
                        {#await octo.request("GET /repos/{username}/{reponame}/stargazers", {
                            username: repo.owner.login,
                            reponame: repo.name,
                        })}
                            <p>Getting Stars</p>
                        {:then stars}
                            <div class="icon-info">
                                <p><i class="nf nf-fa-star"></i> {stars.data.length}</p>
                                <a href={repo.html_url}><p><i class="nf nf-md-github"></i> Open</p></a>
                            </div>
                        {/await}
                    </div>
                {/if}
            {/each}
        {:catch err}
            <p>{err.message}</p>
        {/await}
    </div>
</main>

<style>
    main {
        margin: 0 auto;
    }

    main h2 {
        margin: 0 2rem;
    }

    .repo {
        background-color: rgba(255, 255, 255, 0.075);
        margin: 1rem;
        padding: 0 1rem;

        place-items: center;
        place-content: center;

        display: flex;
        flex-direction: column;
    }

    .repos {
        display: grid;
        grid-template-columns: repeat(3, 1fr);

        margin: 1rem auto;
    }

    .repos p {
        padding: 1rem;
    }

    .icon-info {
        display: flex;
        flex-direction: row;
    }

    i {
        font-size: 1.5rem;
    }

    a {
        color: white;
        text-decoration: unset;
    }
</style>
