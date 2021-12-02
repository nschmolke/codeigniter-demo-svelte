<script>
    import {onMount} from "svelte";
    import PostDataService from "../services/PostDataService";
    import { navigate } from "svelte-routing";
    import { pageNameStore } from '../store.js';

    pageNameStore.update(n => n = "Post bearbeiten");
    let currentPost = {};
    let errors = {};
    let success = false;
    let confirmDelete = false;

    export let id;

    onMount(async () => {
        getPost(id);
    })

    let getPost = (id) => {
        PostDataService.get(id)
            .then(response => {
                currentPost = response.data;
                console.log(response.data);
            })
            .catch(e => {
                if(e.response.status === 404){
                    navigate("/posts", {replace: true});
                }
                console.log(e);
            });
    }

    let updatePost = () => {
        errors = {};
        success = false;

        PostDataService.update(currentPost.id, currentPost)
            .then(() => {
                success = true;
            })
            .catch(e => {
                errors = e.response.data.messages;
                console.log(errors);
            });
    }

    let deletePost = () => {
        PostDataService.delete(currentPost.id)
            .then(() => {
                navigate("/posts", {replace: true});
            })
            .catch(e => {
                errors = e.response.data.messages;
                console.log(errors);
            });
    }
</script>

<main>
    {#if Object.keys(errors).length}
        <div class="relative flex bg-white rounded overflow-hidden p-2 space-x-1">
            <div class="absolute inset-0 border-l-4 border-red-400"></div>
            <div class="flex items-baseline">
                <span class="bg-red-300 bg-opacity-50 rounded-full p-1 mr-3">
                    <svg class="h-6 w-auto text-red-400" fill="currentColor" width="24px" height="24px" viewBox="0 0 24 24" version="1.1">
                        <g stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                            <g transform="translate(12.000000, 12.000000) rotate(-45.000000) translate(-12.000000, -12.000000) translate(4.000000, 4.000000)" fill="currentColor">
                                <rect x="0" y="7" width="16" height="2" rx="1"/>
                                <rect opacity="0.3" transform="translate(8.000000, 8.000000) rotate(-270.000000) translate(-8.000000, -8.000000) " x="0" y="7" width="16" height="2" rx="1"/>
                            </g>
                        </g>
                    </svg>
                </span>
            </div>
            <div class="text-left">
                {#each Object.values(errors) as error}
                    {error}<br>
                {/each}
            </div>
        </div>
    {/if}
    {#if success}
        <div class="relative flex bg-white rounded overflow-hidden p-2 space-x-1">
            <div class="absolute inset-0 border-l-4 border-green-400"></div>
            <div class="flex items-baseline">
                <span class="bg-green-300 bg-opacity-50 rounded-full p-1 mr-3">
                    <svg fill="currentColor" class="h-6 w-auto text-green-400" width="24px" height="24px" viewBox="0 0 24 24" version="1.1">
                        <g stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                            <polygon points="0 0 24 0 24 24 0 24"/>
                            <path d="M9.26193932,16.6476484 C8.90425297,17.0684559 8.27315905,17.1196257 7.85235158,16.7619393 C7.43154411,16.404253 7.38037434,15.773159 7.73806068,15.3523516 L16.2380607,5.35235158 C16.6013618,4.92493855 17.2451015,4.87991302 17.6643638,5.25259068 L22.1643638,9.25259068 C22.5771466,9.6195087 22.6143273,10.2515811 22.2474093,10.6643638 C21.8804913,11.0771466 21.2484189,11.1143273 20.8356362,10.7474093 L17.0997854,7.42665306 L9.26193932,16.6476484 Z" fill="currentColor" fill-rule="nonzero" opacity="0.3" transform="translate(14.999995, 11.000002) rotate(-180.000000) translate(-14.999995, -11.000002) "/>
                            <path d="M4.26193932,17.6476484 C3.90425297,18.0684559 3.27315905,18.1196257 2.85235158,17.7619393 C2.43154411,17.404253 2.38037434,16.773159 2.73806068,16.3523516 L11.2380607,6.35235158 C11.6013618,5.92493855 12.2451015,5.87991302 12.6643638,6.25259068 L17.1643638,10.2525907 C17.5771466,10.6195087 17.6143273,11.2515811 17.2474093,11.6643638 C16.8804913,12.0771466 16.2484189,12.1143273 15.8356362,11.7474093 L12.0997854,8.42665306 L4.26193932,17.6476484 Z" fill="currentColor" fill-rule="nonzero" transform="translate(9.999995, 12.000002) rotate(-180.000000) translate(-9.999995, -12.000002) "/>
                        </g>
                    </svg>
                </span>
            </div>
            <div class="text-left">
                Der Post wurde erfolgreich bearbeitet!
            </div>
        </div>
    {/if}
    {#if currentPost}
        <div class="shadow overflow-hidden sm:rounded-md">
            <div class="px-4 py-5 bg-white sm:p-6">
                <div class="grid grid-cols-6 gap-6">
                    <div class="col-span-6">
                        <div class="text-base font-medium text-gray-900">
                            Titel
                        </div>
                        <input type="text" name="title" bind:value={currentPost.title} class="mt-1 focus:ring-primary-500 focus:border-primary-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                    </div>
                    <div class="col-span-6">
                        <div class="text-base font-medium text-gray-900">
                            Text
                        </div>
                        <div class="mt-1">
                            <textarea id="text" name="description" bind:value={currentPost.description} rows="3" class="shadow-sm focus:ring-primary-500 focus:border-primary-500 mt-1 block w-full sm:text-sm border border-gray-300 rounded-md" placeholder="Dies ist der Anfang deiner Story..."></textarea>
                        </div>
                    </div>
                    <div class="col-span-6">
                        <fieldset>
                             <div class="text-base font-medium text-gray-900">
                                 Veröffentlichung
                             </div>
                            <div class="mt-4 space-y-4">
                                <div class="flex items-center">
                                    <input id="published-no" name="status"
                                           bind:group={currentPost.status}
                                           value={"Draft"} type="radio"
                                           class="focus:ring-primary-500 h-4 w-4 text-primary-600 border-gray-300"/>
                                    <label for="published-no"
                                           class="ml-3 block text-sm font-medium text-gray-700">
                                        Entwurf
                                    </label>
                                </div>
                                <div class="flex items-center">
                                    <input id="published-yes" name="status"
                                           bind:group={currentPost.status}
                                           value={"Published"} type="radio"
                                           class="focus:ring-primary-500 h-4 w-4 text-primary-600 border-gray-300"/>
                                    <label for="published-yes"
                                           class="ml-3 block text-sm font-medium text-gray-700">
                                        Veröffentlicht
                                    </label>
                                </div>
                            </div>
                        </fieldset>
                    </div>
                </div>
            </div>
            <div class="px-4 py-3 bg-gray-50 text-right sm:px-6 grid grid-cols-2">
                <button on:click={ updatePost } class="mr-2 inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-primary-600 hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary-500">
                    Speichern
                </button>
                <button on:click={ () => {confirmDelete = true;} } class="ml-2 inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-red-600 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500">
                    Löschen
                </button>
            </div>
        </div>
    {/if}

    {#if confirmDelete}
        <div class="fixed z-10 inset-0 overflow-y-auto" aria-labelledby="modal-title" role="dialog" aria-modal="true">
            <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
                <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" aria-hidden="true"></div>
                <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
                <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
                    <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                        <div class="sm:flex sm:items-start">
                            <div class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
                                <svg class="h-6 w-6 text-red-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
                                </svg>
                            </div>
                            <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                                <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-title">
                                    Post löschen
                                </h3>
                                <div class="mt-2">
                                    <p class="text-sm text-gray-500">
                                        Bist du sicher, dass du den Post löschen möchtest? Diese Aktion kann nicht widerrufen werden!
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
                        <button type="button" on:click={deletePost} class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 sm:ml-3 sm:w-auto sm:text-sm">
                            Löschen
                        </button>
                        <button type="button" on:click={ () => {confirmDelete = false;} } class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">
                            Abbrechen
                        </button>
                    </div>
                </div>
            </div>
        </div>
    {/if}
</main>
