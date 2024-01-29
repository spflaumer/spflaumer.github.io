<script type="module">
    import { Octokit } from "octokit";

    const octo = new Octokit({});

    const user_req = octo.request("GET /users/{username}", {
        username: "spflaumer",
    });
</script>

<header>
    <a href="/"><h1>GitHub Profile</h1></a>
</header>

<main>
    <aside class="profile">
        {#await user_req}
            <!-- svelte-ignore a11y-img-redundant-alt -->
            <img class="profile-picture" alt="profile picture"/>
            <p class="profile-name">Simon&nbsp;Peter&nbsp;Pflaumer</p>
            <span class="profile-urls">
                <a href="/about"><p>About me</p></a>
            </span>
            {:then user}
            <!-- svelte-ignore a11y-img-redundant-alt -->
            <img class="profile-picture" src={user.data.avatar_url} alt="profile picture"/>
            <p class="profile-name">Simon&nbsp;Peter&nbsp;"{user.data.login}"&nbsp;Pflaumer</p>
            <span class="profile-urls">
                <a href={user.data.html_url}><p><i class="nf nf-md-github"></i> GitHub</p></a>
                <a href="/about"><p><i class="nf"></i>About me</p></a>
            </span>
        {/await}
    </aside>

    <slot />
</main>

<style>
    header {
        padding: 1rem 2rem;
    }

    i {
        font-size: 1.5rem;
    }

    a {
        color: white;
        text-decoration: unset;
    }

    main {
        display: flex;
        flex-direction: row;
    }

    .profile {
        width: 25%;
        flex-grow: 0;
        flex-shrink: 1;
        flex-wrap: nowrap;

        display: flex;
        flex-direction: column;
        place-items: center;

        margin: 1rem;
        padding: 1rem;
    }

    .profile-picture {
        border-radius: 100%;
        min-width: 200px;
        min-height: 200px;
        max-height: 50%;
        max-width: 50%;
    }

    .profile-urls {
        display: flex;
        flex-direction: row;
    }

    .profile-urls p {
        padding: 0 1rem;
    }

    .profile-name {
        padding: 0.5rem 1rem;
    }
</style>
