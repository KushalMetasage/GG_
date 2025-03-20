<script>
	import '@evidence-dev/tailwind/fonts.css';
	import '../app.css';
	import { EvidenceDefaultLayout } from '@evidence-dev/core-components';
	export let data;
</script>

<div class="flex flex-col min-h-screen w-full">
    <!-- Main Content Area with Sidebar and Breadcrumbs Hidden -->
    <EvidenceDefaultLayout {data} hideBreadcrumbs = true hideSidebar = true logo = /globalgreen.png neverShowQueries = true 
    hideTOC = true>
        <slot slot="content" />
    </EvidenceDefaultLayout>

    <!-- Footer Navigation Bar -->
    <footer class="bg-gray-900 text-white py-4 px-6 shadow-md fixed bottom-0 left-0 w-full">
        <ul class="flex justify-center gap-6 text-sm">
            <li><a href="/" class="hover:text-green-400">🏠 Home</a></li>
            <li><a href="/gg_comment_business" class="hover:text-green-400">📈 Business Comments</a></li>
            <li><a href="/income_statement_overview" class="hover:text-green-400">📑 Income Statement Overview</a></li>
            <li><a href="/gg_mmr_bs" class="hover:text-green-400">📊 Balance Sheet</a></li>
            <li><a href="/cashflow" class="hover:text-green-400">💰 Cash Flow Analysis</a></li>
            <li><a href="/gg_plantutilization" class="hover:text-green-400">🏭 Plant Utilization</a></li>
            
            
            
        </ul>
    </footer>
    
</div>