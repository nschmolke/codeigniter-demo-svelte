<script>
	import Navbar from "./components/Navbar.svelte";
    import { Router, Route, Link } from "svelte-routing";
    import Home from './views/Home.svelte';
    import Posts from './views/Posts.svelte';
    import EditPost from './views/EditPost.svelte';
    import CreatePost from './views/CreatePost.svelte';
	export let url = ""; //This property is necessary declare to avoid ignore the Router
</script>

<Router url="{url}">
    <Navbar/>
    <main>
        <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
            <!-- Replace with your content -->
            <div class="px-4 py-6 sm:px-0">
                <Route path="posts" component="{Posts}"/>
                <Route path="posts/create" component="{CreatePost}"/>
                <Route path="posts/:id" let:params>
                    <EditPost id="{params.id}"/>
                </Route>
                <Route path="/" component="{Home}" />
            </div>
            <!-- /End replace -->
        </div>
    </main>
</Router>


<style global lang="postcss">
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
</style>
