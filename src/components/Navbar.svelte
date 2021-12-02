<script>
    import { Link } from "svelte-routing";
    import logo from '../images/ba-logo-monochrome.svg';
    const logoEncoded = `data:image/svg+xml,${encodeURIComponent(logo)}`;
    import { pageNameStore } from '../store.js';

    let pageName;

    const unsubscribe = pageNameStore.subscribe(value => {
        pageName = value;
    });

</script>
<!-- This example requires Tailwind CSS v2.0+ -->
<nav class="bg-gray-800">
    <div class="max-w-7xl mx-auto px-2 sm:px-6 lg:px-8">
        <div class="relative flex items-center justify-between h-16">
            <div class="flex-1 flex items-center justify-center sm:items-stretch sm:justify-start">
                <div class="flex-shrink-0 flex items-center">
                    <img class="lg:block h-8 w-auto" src={logoEncoded} alt="Workflow">
                </div>
                <div class="block sm:ml-6">
                    <div class="flex space-x-4">
                        <!-- Current: "bg-gray-900 text-white", Default: "text-gray-300 hover:bg-gray-700 hover:text-white" -->
                        <Link to="/" class="text-gray-300 hover:bg-gray-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium {pageName === 'Home' ? 'bg-gray-900 text-white' : '' }">Home</Link>

                        <Link to="/posts" class="text-gray-300 hover:bg-gray-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium {pageName.includes('Post') ? 'bg-gray-900 text-white' : '' }">Posts</Link>

                    </div>
                </div>
            </div>
            <div class="absolute inset-y-0 right-0 flex items-center pr-2 sm:static sm:inset-auto sm:ml-6 sm:pr-0">
                <span class="text-white">SVELTE Projekt</span>
            </div>
        </div>
    </div>
    <header class="bg-white shadow">
        <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
            <h1 class="text-3xl font-bold text-gray-900">
                { pageName }
            </h1>
        </div>
    </header>
</nav>
