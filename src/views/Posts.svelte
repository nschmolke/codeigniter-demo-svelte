<script>
    import PostDataService from "../services/PostDataService";
    import {onMount} from "svelte";
    import { Link } from "svelte-routing";
    import { pageNameStore } from '../store.js';
    import {navigate} from "svelte-routing";

    pageNameStore.update(n => n = "Alle Posts");

    let posts = [];

    onMount(async () => {
        retrievePosts();
    })

    let retrievePosts = () => {
        PostDataService.getAll()
            .then(response => {
                posts = response.data;
                console.log(response.data);
            })
            .catch(e => {
                console.log(e);
            });
    }

    let refreshList = () => {
        retrievePosts();
    }
</script>

<div class="container max-w-7xl mx-auto">
    <div class="flex flex-col">
        <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
            <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
                <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
                    <table class="min-w-full divide-y divide-gray-200">
                        <thead class="bg-gray-50">
                        <tr>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                ID
                            </th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                Titel
                            </th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                Status
                            </th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                Erstellt am
                            </th>
                            <th scope="col" class="relative px-6 py-3 text-right">
                                <button class="text-gray-800 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-base font-medium" on:click={refreshList}>Aktualisieren</button>
                                <button class="text-white bg-primary-600 hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary-500 px-3 py-2 rounded-md text-base font-medium" on:click={() => {navigate("/posts/create", {replace: true});}}>Erstellen</button>
                            </th>
                        </tr>
                        </thead>
                        <tbody class="bg-white divide-y divide-gray-200">
                        {#if posts}
                            {#each posts as post }
                             <tr>
                                <td class="px-6 py-4 whitespace-nowrap">
                                    <div class="text-sm text-gray-900">{ post.id }</div>
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap">
                                    <div class="text-sm text-gray-900">{ post.title }</div>
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap">
                                    {#if post.status === "Draft"}
                                        <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800">
                                          Entwurf
                                        </span>
                                    {:else}
                                        <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800">
                                          Veröffentlicht
                                        </span>
                                    {/if}
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                                    { post.created_at }
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                                    <Link to={"/posts/" + post.id} class="text-gray-800 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-base font-medium">Bearbeiten</Link>
                                </td>
                            </tr>
                            {/each}
                        {:else}
                            <p class="loading">loading...</p>
                        {/if}
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</div>
